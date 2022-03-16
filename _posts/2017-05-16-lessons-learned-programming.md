---
layout: post
title: Lesson learned after programming full-time for a year.
---
It's been more than a year now since I have been programming full-time. And I
would like to reflect on the kind of problems that I encounter most often.

I don't actually want to talk about specific problems that I have solved
rather about one meta-problem that have occurred time and time again.

The most common meta-problem is to get the code that was written to do
almost what I want it to do but not quite to do exactly what I want it to do.

Whether it is a change in requirements, or me developing some component that
relies on the existing infrastructure, or me spotting that two independent
components that are doing exactly the same thing and should actually be
refactored to be unified as to avoid duplication: there is always something out
there in the codebase that is almost there but not quite. So 95% of the code is
already there, but the last 5% often takes days of work to write and plug in.

And if you read "Code Complete: A Practical Handbook of Software Construction",
top voted pick of ["What is the single most influential book every programmer
should read?" on StackOverflow][most-influential], you will know that a big
chunk of the book advocates planning ahead exactly so that this sort of
scenarios don't happen often: "If you can avoid writing 70 percent of your code
by planning ahead, do it!", says the book. More quotes from the book on the
subject: "It takes only a few large programs to learn that you can avoid a lot
of stress by planning ahead.", "From a technical point of view, planning means
understanding what you want to build so that you don't waste money building the
wrong thing.".

I, however, have grown to be pessimistic about the value of such planning. Or at
least elaborate planning. My planning now solely consists of decoupling as much
as possible in an anticipation of unforeseen changes (the book will also call
this strategy planning, however it appears cheating to me: might as well just
say "write good software" at this stage).

Seriously though, if you can foresee exactly which components will be changed
months in the future - I think you are wasting your talents writing software.
You should do something more significant with such forecasting abilities.
Every time I modified either my own code or somebody else's I always know the
reason why it was written the way it is written. And it was always done so for
good reasons: it was written with a goal X in mind, not Y. To be slightly more
concrete, this is a somewhat common situation.

{% highlight go %}
func validateAndPostEvent(event *Event, endPoint *Url) {
	// validates the event against a schema
	// ...
	// posts it
}
{% endhighlight %}

Validation and posting is coupled in the above situation because there's was no
other reason to validate the event separately from posting at the time when the
code was written. And now that I am writing say, a static schema verifier, I
need that validation part of the code, but I need to figure out how to decouple
it away from posting.

Another occurrence is:

{% highlight python %}
def score_bunch_of_rows(csv_in, csv_out, model):
    # read csv
    # process csv
    # score using model.score
    # write csv out
{% endhighlight %}

However *this time* I need to score using `model.contribtutions` function, so I
want to reuse most of the code above, but change just 1 line in the middle.
However read_csv, process_csv, model.score & write csv are all coupled in the
above function. The reason it is all coupled because the author, me, didn't
think at the time that I would need to call `.contributions` in the above
function.

*These are toy examples that make it appear that decoupling from the start was
a negligible amount of effort, but as I already mentioned, it often takes me
days to tear certain functions apart: they were coupled for good reasons at the
time that are hard to communicate. Real world software is complex.*

------------

Like I already mentioned above, my software writing strategy has steered
towards: try to decouple code components as much as possible right from the
start even if it's not clear to you now how these functions could potentially
be reused right now. Concretely it means: if your function takes a struct but
only uses 4 fields of the struct, then create a function that operates on 4
fields only and another function that takes a struct but calls the first
function instead with the required fields. If your function requires an object
with `.predict` method, you might as well just take the `.predict` method
rather than an instance of an object as function argument. This way the callee
doesn't have to mock the interface of the entire class just to so that you get
to call `.predict`.

OOP-wise it means having classes as lean as possible. If you have, for
instance, the following abstract class:

{% highlight python %}
class PersonRecord(abs.ABC):
    def __init__(firstName, lastName, email, phoneNumber):
        pass

    def phoneCountry(self):
    	# compute phoneCountry from self.phoneNumber

    @abc.abstractmethod
    def fullName(self):
    	# insist derived classes implement this
{% endhighlight %}

This means you now can't call `phoneCountry` function on a free text telephone
number to find what country the phone number is from without creating an
instance with irrelevant details such as `firstName`, `lastName`.
`.phoneCountry` and names are coupled even though they should not be. In
addition, you have to hunt around for a concrete implementations that might
have their own idiosyncratic requirements in order to call our `.phoneCountry`
method. Not cool. (Another way to think about it: every time you add a field to
a class you create an implicit dependency between *all* methods of that class
and the field! Unless *all* of the methods will make use of the field, you just
created an unnecessary dependency.)

And this is the meta-lesson that I learned. Steer towards fewer dependencies
between functions and classes. Only require what you need in a function. Only
put things into a struct when you are very sure these things will always be
used together. Avoid creating classes if you can. Tada.

P.S. I suspect none of this is new, however I don't know the name for this
technique.


[most-influential]: http://stackoverflow.com/questions/1711/what-is-the-single-most-influential-book-every-programmer-should-read

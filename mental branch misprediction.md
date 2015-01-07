#Keep your coder's mind at full speed: avoid mental branch mispredictions

##Brains and CPUs

One can make an analogy of the human brain and a CPU, but
never is the analogy more valid than in the case of a programmer
*reading* source code.

##The coder's mind

When reading source code, trying to understand what the code does,
the mind of a trained programmer works much like a CPU.

We're "following" (executing) the source code in our minds, 
trying to determine what the program does.

*..then calls this function, stores result here, if it's less than 10, then it...*

##Branch Misprediction

Do you know what a "branch misprediction" is ?

Branch misprediction is an interesting artifact of CPU's execution, 
since it effects can "bubble" up to high-level languages.

They say "A picture is worth a thousand words", well this explanation is  illustrated with a picture, and as today it seems to worth more than ten thousands upvotes. 

Please *go now* and read [this fantastic answer](http://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-an-unsorted-array/11227902#11227902) 
at StackOverflow. 


##MBM: Mental Branch Misprediction

Now, merging both concepts, a **Mental Branch Misprediction**
is what occurs in the coder's mind when the source code language we're reading, has some specific constructions causing our minds to "mispredict" execution,  and lose our train of thought.

An example of this kind of syntax is the "if modifier"/"if postfix form" 
of Ruby & also CoffeeScript (very nice languages)

Here's a example from CoffeeScript main page:

    number   = 42
    opposite = true

    # Conditions:
    number = -42 if opposite

*... ok, number=42, opposite=true, now number= - 42... WAIT!...  only IF "opposite" then number = - 42...*

The "WAIT" was branch misprediction on your brain, you had to stop, pull back the train, and parse again.

It was a simple statement, but the complex the code, the more to backtrack. Read the following code:

    number   = 42
    opposite = true
    number = complexFunction.call(obj, oldValue, oldValue+100, baseValue*getFactor()) unless opposite

From this we can derive a simple rule to avoid mental branch misprediction: all conditionals evaluations should precede conditionally executed statements.

JavaScript (another wonderful language) does not suffer from this, 
but it presents a very complex branch prediction cases when you use async callbacks and closures.

Example: (node.js)

    fs.readFile('test', function(err,data){
        console.log(data.toString());
        if (data.length>1000){ 
            console.log('more than a thousand chars')
        };
    });
    console.log('data read started');

Again a very simple program, but, follow me while we try to use the tracks junction analogy for this. Suppose the the train is the execution thread:

>The train passes by the junction and go straight saying "read this file please". *At a future time*, the train *magically reappears running at full speed*, but now in the diverging track -just after the junction-, then follows this short track, and disappears to nowhere at the end of it.

That was just a funny example. Async is really great, and the perfect tool for a myriad of cases, but sometimes you do not want or need async, and a callback only adds a magic future branch to the source code, and trains appearing and disappearing to nowhere. 

It can be even worst, when enough async callbacks accumulate you get 
the "pyramid of doom" and end up in "callback hell".

So: **"async is really good", but not all the time**

I love to code, I've been programming for a lot of years, and I've reached a stage where I really appreciate simplicity, and know how hard is to achieve it. (this blog platform, svbtle, is a good example of "hard to achieve simplicity")

I am designing a language: [LiteScript](https://github.com/luciotato/LiteScript), -while coding a PEG based compiler/transpiler for it-. One of the design objectives is the kind of simplicity that lets a coder's mind flow.

The previous async example can be made much more readable
-without losing the async advantages- by using ES6 generators and LiteScript's "yield until":

For example, in the nice function below, the callback magic junction is gone and your train of thought can go at full speed.

    nice function sequentialRead
        console.log('data read started');
        var data = yield until fs.readFile('test')
        print data.toString()
        if data.length>1000, print 'more than a thousand chars'

Please check [LiteScript](https://github.com/luciotato/LiteScript). It is open source, brand new, still in beta, and I'll appreciate collaborations.

A teaser: *write the following in pure js (node)*

#####get google.com IPs, then reverse DNS (in parallel)
LiteScript code:

    global import dns, nicegen

    nice function resolveAndParallelReverse

        try

            var domain = "google.com"

            var addresses:array = yield until dns.resolve domain

            var results = yield parallel map addresses dns.reverse 

            for each index,addr in addresses
                print "#{addr} reverse: #{results[index]}"

        catch err
            print "caught:", err.stack

    end nice function

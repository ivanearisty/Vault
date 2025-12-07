## Intro

We're basically done with the semester! I know we call these homeworks but tbh they're more like little projects. 

At the end of the day, this entire course is based on group work.

I hope that by now you're understanding a little more what we actually care about in this course. We're so lenient with the use of AI because the implementation, at the end of the day, doesn't matter.

Inneficient methods, slow data access, complicated implementations; these details are not what the course is all about.

We only care about putting you in a spot that will make you a better engineer in the field. If you use python, you can be sure that any enterprise will be using a package manager that's not pip; will have strict rules around how to commit, push, and rebase things; they'll have hyper-strict code quality checkers; your senior engineers will be brutal when you make incorrect design choices; everyone will hate you if you break the build (ci); and every single thing you do has to be documented. 

That's what this course is really about, and I think you've come a long way.

The differences in care between HW1 and HW2 are big, and I hope that you're finding better ways to prototype and work with your team.

Before starting this assignment, think about what has worked and what hasn't and come up with solutions. 

I want to talk about an example. Look, now you know how to make RUFF work properly, this time around maybe run ruff every time you make a new method.

I do this myself. I work on a lot of random projects, and I always set up ruff and mypy and make python be hyper-strict.

In fact, I'll give you this, which I put on my vscode configuration which is what I swear by when doing python work nowadays:
```json
SETTINGS.JSON:

{
"python.analysis.typeCheckingMode": "strict",
"python.analysis.inlayHints.variableTypes": true,
"python.analysis.inlayHints.functionReturnTypes": true,
"python.analysis.autoFormatStrings": true,
"python.analysis.enablePytestSupport": true,
"python.analysis.supportDocstringTemplate": true,
"python.analysis.typeEvaluation.strictListInference": true,
"python.analysis.typeEvaluation.strictSetInference": true
}
```

When working with ML models, I have found it very annoying how most ipynbs are not typed at all, it makes it very confusing for others to understand. 

You'll routinely see things like {insert ml examples}, which, when you're deep in the model, you WILL get lost in the sauce and not know sometimes what item you're actually dealing with. Is it a model? a feature? a layer? seriously, these things are meant to help you write better python code, but I hope that these concepts will also translate into other areas of your professional software development journey.

I think seniors are very impressed when you work with care, when you make tight interfaces and respect the fact that other developers have to also understand your code. Seriously, I think there is nothing more clean than breaking down a problem into a set of methods that need to be implemented in an interview. I remember one time that I needed a function that calculated the binomial something for a value. I had no idea how to implement it, I just made a method and the interviewer didnt even fault me for it. The bigger picture is always what is most important.

I hope you also took a look at the rubric and realized how we're actually grading you. To be honest, we did not run a single line of your code for HW2. We trust you that if it passes CI that it works (i mean, you're running e2e tests). What we did in so far as checking the validity was running uv sync and uv build. Really! That's it. ngl, for hw3 we will be trying it out as you will shortly see. but yeah, the core idea of this course has always been that you.
:skip-help: true
:css: css/stylesheet.css
:title: How to diff XML

.. footer::

    .. image:: images/britecore.png

----

Moving Big Projects to Python 3
===============================

.. class:: name

    Lennart Regebro

.. class:: location

    EuroPython, 2019

.. note::

    My name is very hard to pronounce.
    In fact, almost every time somebody asks how my last name is pronounced,
    I get self conscious and mispronounce it.

    So just call me Leonard.

----

.. class:: blurb

BriteCore is the leading technology platform for modern insurance providers.
Fully managed through Amazon Web Services cloud, BriteCore is continually
updated to guarantee maximum security, efficiency, and durability at scale.
Over 45 Carriers, MGAs, and InsureTechs rely on BriteCore for their core, data,
and digital needs.

.. note::

    I work for BriteCore.

    We do the type of software that insurance companies use to deal with
    insurance policies and claims.

    We are over a 100 people right now, we work remotely, and yes, we are hiring.
    If you are looking for a job and want to work remotely, talk to me!

----

.. image:: images/shoobx.png
    :width: 600px

.. note::

    But I should also shoutout to Shoobx, where we successfully moved
    a large and insanely complex system to Python 3 last year.

----

.. image:: images/stoneage.jpg
    :width: 800px

.. note::

    So, let's go back back to the stoneage,
    when your company created some web based application,
    and you did such a good job that it's still running!

----

.. image:: images/grok.png
    :height: 600px

.. note::
    This is you, and this is your webframework.

    You are running it on some old version of probably Django or Web2py.
    Possibly Turbogears, maybe even Zope!

    Let's fast forward to the current time.

----

.. image:: images/sirrobin.jpg
    :width: 100%

.. note::
    Which apparently is the medieval.
    And you have been bravely running away from Python 3 for years.

    But, you can't run any longer. Time to face the monster.
    But don't fear the syntax errors, those are the easy parts, actually.
    The hard part is getting your old system into a state where it's easy to port.

----

Step 1: Stop being a fire department!
=====================================

.. image:: images/firefighting.jpg
    :width: 80%

.. note::
    Many large organizations are constantly putting out fires.
    That's not a good situation to port to Python 3,
    because if the changes you do as a part of normal development breaks production,
    and you need to put out that fire,
    then moving to Python 3 is going to start new fires.
    Also, all your developers will be too busy putting out fires to port things.

    So the first thing you need to do is to get out of firefighting mode.
    And that in itself is a whole talk,
    and I'm not the one to do that talk anyway.
    But I'll quickly mention a few things I've seen DevOps do to fix this.

----

Continuos integration

Use staging servers

Automatic deployment

Monitoring

Increase test coverage

.. note::

    Some of these are optional, some are not.

    You HAVE to have continuos integration to move to Python 3.
    I'll talk more of that later.
    Switching to Python 3 without a staging setup would also be insane.

    Automatic deployment is a nice thing to get out of firefighting mode.
    Deployment of a new release of the software should just be a push of the button.
    Extra points if master is released and pushed to staging every night.

    Monitoring is good, and tests also help for stability.

----

Isolated production environment
===============================

.. note::

    And there are some Python specific things we can do to harden production.
    One thing is to run in some sort of isolated environment.
    This typically means a virtualenv, or if you run Plone, a buildout.

    Containers are in now, that helps isolating,
    so you don't get weird interactions with new versions of OS packages.

----

Docker
======

.. note::

    This is probably going to be obvious to most of you,
    but I've just realized this the last few months,
    so I'll mention it because it's new to me!

    If you, every time your package requirements change,
    build a new docker image, including the virtual environment for the servers,
    then if some new requirement change creates conflicts,
    you don't notice that during deployment, but while building the packages!
    Yay, deployment didn't mess up production,
    it stopped before production was even touched!

    In addition, you can then use those images on CI, and even to develop on,
    so you know that developers run the same environment as production!

----

.. image:: images/mindblown.gif
    :width: 100%

----

.. image:: images/coffeebreak.jpg
    :width: 100%

.. note::

    So, your firefighters now can take it easy.

----

Stage 2: Planning
=================

.. note::

    When the firefighting is done, it's time to actually think about porting,
    and do some planning. And then I have three questions for you.

----

Can you stop adding features?
=============================

.. note::

    It depends very much on your business
    if you can take a time out from adding features to do the porting or not.
    If you can, you can put everyone on doing it, which is great and will be faster.
    It also means everyone feels involved.
    But it still will take a few weeks at least. Maybe longer.
    So can you stop adding features and stop firefighting that long?

----

Do you have magic?
==================

.. note::

    And if some parts of your code is doing deep magic, it can be very hard to port.
    And then the few of your Python gods that actually understand that code,
    will be busy with that, when everything else already works.
    Or, it's so deeply integrated in the code that nobody can actually port their bits
    until that deep magic is fixed.
    In both of those cases, everyone that are supposed to port to Python 3 will be blocked.

----

How big is your team?
=====================

.. note::

    The famous mythical man-month remains mythical also with Python 3.
    Putting 50 developers on porting at the same time will not work.
    Ten isn't a problem, you can synchronize that, at least if they are
    in the same office. Maybe even 20, but no more than that.
    If your system is already split into multiple separate services
    that run separately, then you can probably put each team on porting their bit separately,
    so then you are already ahead of the game, but most of these big systems are monoliths.

----

Strategy: One big push!
=======================

.. note::

    You don't have deep magic.
    You can stop adding features.

    Then you can do it all in one go.

----

One big push: Benefits
======================

Takes less time

Less work in total

You can aim directly for Python 3 code

----

One big push: Drawbacks
=======================

High risk

All other work stops

----

Strategy: Slow and steady
=========================

.. note::

    For those reasons, porting big projects to Python 3 is usually done slowly and carefully.
    You will port the code to code that runs on both Python 3 and Python 2,
    even though you run it on Python 2.
    And then, one day, you can finally switch and run it on Python 3.

    This way is common, slow, more time intensive.
    And also, if you have a 100 developers, getting all 100 onboard as one will be hard.
    Since we are so many at BriteCore, and in addition everyone are remote,
    this is what we are going to do.


----

Slow and steady: Benefits
=========================

Low risk

Doesn't disrupt normal operations

----

Slow and steady: Drawbacks
==========================

More work

Longer total time

You need dual version support

----

Strategy: Mix it up!
====================

.. note::

    If you have a development team small enough to fit into one big country house,
    you can start with a Python 3 sprint for all the developers,
    but not aim for Python 3, but aim for a Python 2/3 compatible code.
    That way, when they come back half done, you can switch to have a dedicated team do the last bit,
    or just have people do it when there is no critical work.

    This is what we did at Shoobx.

----

Mix: Benefits
=============

Low risk

Only disrupts normal operation briefly

Everyone gets onboard and feels involved

----

Mix: Drawbacks
==============

You need dual version support

Still slow

----

Time to start preparing
=======================

----

Pin all versions
================

.. note::

    To make sure that you know what you install,
    you should pin all versions of all packages.
    pip unfortunately has no flag I can find to require this.
    What you can do is to add hashes to the requirements,
    if you add one hash, it will require hashes for all packages,
    effectively making sure no new requirement goes unpinned.

    This makes for huge requirements files with loads of hashes in them.
    But it also adds extra security.

    Another way to do this would be to verify in the install script
    that what you installed matches the requirements file,
    by f ex comparing your pip freeze output with the requirements file.
    That way, you would get an error if you change one package
    that introduces new dependencies.

    And once you have a production environment that is stable,
    then it's time to move on to preparing for Python 3.
    And the first thing you should do there is to

----

Increase test coverage (again)
==============================

.. note::

    Add even MORE tests.
    And do coverage, so you know how many lines of code you are testing.

    What percentage of test coverage you want is really a matter of opinion.
    But it is very good to cover a line, because lines that aren't covered may
    contain hidden Python 2 code.

    100% is awesome, but is likely practically unobtainable.
    90-95% would be my target. You can bridge the gap somewhat by
    carefully reading all non-covered lines and looking for Python 2 syntax
    on the non-covered lines, at some point that becomes easier than writing a test.

----

Mock gotchas
============

.. note::

    For Python 3 it really is line coverage we are looking for.
    So mocking out most of the calls in a function is perfectly fine.

    UNLESS, your mocking adds a method or function that no longer exists in Python 3.
    Then you shot yourself in the foot, so be careful with that.
    Integration testing is therefore also needed, you can't have just unit tests.

----

Upgrade dependencies
====================

Upgrade all packages

Replace or port anything that isn't Python 3 compatible

.. note::

    Make sure you have the latest Python 2 compatible version of all your dependencies.
    Then make sure all your dependencies are Python 3 compatible.
    You may have to replace, or worst case, port, some of your dependencies at this point.

    This stage can take a significant time, especially if you have not been keeping
    your dependencies up to date.

----

Setup your testing for Python 3
===============================

.. note::

    It's now time to start running your tests under Python 3,
    and this will obviously always fail.
    If you have decided to start with a big sprint where everyone is helping,
    you need to simply start digging into fixing those tests.

    But if you are doing this gradually, there is a significant risk that
    people introduce incompatible code faster than you can fix it.

----

.. image:: images/backwards.gif
    :width: 100%

.. note::

    In that case you will never finish.
    So, to stop that you need to do some sort of magic with your tests.

----

Call in the CI Gurus
====================


.. note::

    The best way to do this is to let your CI system keep track
    of which tests that once DID pass under Python 3,
    and if a test that should pass no longer passes under Python 3, flag the test run as failed.
    But you can't require ALL tests to pass under Python 3 initially,
    because then all your builds will fail and you can never merge anything.

    At Shoobx we had tests that were very big.
    The browser integration tests would typically run over night.
    Running all those tests also under Python 3 would require twice the computing power,
    making the tests runs twice as expensive. Not much fun.

    What the clever people who did the CI at Shoobx did,
    was to use the coverage information to see what tests covered which lines,
    and when a pull request was done, we could use the diff to see
    what lines and hence what tests were affected.
    Then, for that PR we could run only the tests affected.

    For master, of course, you would run everything.
    But this lowered the time it would take to get integration tests on a branch run.
    Unit tests of course are fast, always run all of them.

----

Make your dev environment support Python 3
==========================================

.. note::

    Do you have scripts to set up a development environment?
    Or are you using docker? Something else?
    In any case, that environment should be able to be built under Python 2 or Python 3 or both.
    Any build scripts you have need to support both versions,
    any auxilary scripts you have also need to do that.

    At BriteCore we for example have scripts that help you copy test databases,
    set up docker images etc. It's usually easier if they support Python 2 and Python 3
    first, so you don't have to keep two environments going.
    Sometimes the dev help scripts already run in a separate virtualenv,
    and then you might be able to port them later.
    But then again, if they are separate, you might want to do them first as practice!

----

Write data migration tests
==========================

Do you get text strings when you expect text strings?

Are non-ascii chÃ©racters interpreted corrÃ¶ctly?

Are you loading data from disk at some point?

Are you using pickles? ARE YOU?

.. note::

    You should take data that is created with the software running on Python 2,
    and write tests to make sure you get the right data in Python 3.

----

Time to start fixing!
=====================

.. note::

    Next step is to fix all import and syntax errors,
    so your test runner can actually find the tests.

----

Modernize
=========

.. note::

    Modernize is a set of 2to3 fixers that generate backwards compatible code, mostly with six.
    I would recommend run all the Modernize fixers on your code, one by one, and review those changes.
    Because fixers aren't perfect.

    On smaller codebases I completely recommend just running Modernize once on everything
    and see if the tests still run. But on any larger code base it won't still run,
    and with the massive changes you get, it can be hard to figure out what went wrong.
    It's better to do it carefully.

    You might think you want to run it file by file instead, but there's a reason to not do that.

----

Import errors everywhere
========================

.. note::

    Your first errors will be import errors.
    That's because some module with have a syntax error,
    and the modules trying to import from that module will have some sort of syntax error.
    So the first thing you want to do is fix those syntax errors.
    And if you are then running ALL fixers on a file with syntax errors,
    you might end up introducing another error, meaning you still get the same import errors.

----

One fixer at a time
===================

.. note::

    In the beginning, until you get rid of import errors,
    you might even want to run one fixer at a time on one file at a time.

    Once you have gotten rid of all import errors,
    which also means you are rid of all the syntax errors,
    then you can start porting for real,
    because now you will have tests that can fail, or pass.

----

Port port port
==============

The book is outdated, but free!

.. note::

    If you didn't write the migration tests before, do it now.

----

Push to staging
===============

Test it carefully, manually, with real data

----

Push to production
==================

Be prepared to fall back if possible!

.. note::

    If you have the possibility to move customers one by one, do that.
    Start small, work yourself up.

    If you have to migrate the database, you may not be able to go back to Python 2,
    so in that case you need to be extra careful.

----

Celebrate!
==========

.. image:: images/party.gif
    :width: 100%

----

Clean up
========

.. image:: images/cleanup.jpg
    :width: 100%

.. note::

    And then clean up.
    Actually, that picture is a misrepresentation.

----

.. image:: images/funclean.jpg
    :width: 100%

.. note::

    Because this is the fun bit.
    This is where you can go through the code and remove loads of old cruft.
    See it as an opportunity to just prettify the code.

----

.. image:: images/done.gif
    :width: 100%

----

Questions?
==========

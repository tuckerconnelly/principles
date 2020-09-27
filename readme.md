# Tucker's Principles

Here is a list of principles, mostly programming related. Hopefully they're helpful to you.

I'll update these from time to time as I introspect and learn. Feel free to leave comments in the issues or on my [Twitter](https://twitter.com/tuckerconnelly).

If you have your own list, please share :)


## Web Programming

 * Every line of code should be used; delete it if not.
 * Follow the test trophy. https://twitter.com/kentcdodds/status/960723172591992832
 * If there’s a bug, write a test first.
 * Function arguments and props should be in affirmative.
 * Divide code into abstract (open-source-able) and concrete (application code)—no middle abstractions.
 * Use extra variables or comments where it will make code more clear for the next person.
 * Look to existing, popular libraries for ideas on architectural boundaries that actually work.
   * For example, `semantic-ui` for abstract component props.
 * Function names follow {verb}[{noun}] format.
 * If possible, make the whole database schema first, to avoid tedious migrations later.
 * Clearly denote hacks; make them awkwardly obvious with variable names and comments.
 * The most likely cause of the bug is you, not the tools (from Pragmatic Programmer, "SELECT isn't broken").
 * Do one thing at a time; handle one domain concept at a time.
 * Create clear sections within a file or function with, if necessary, over-the-top comments.
 * Write idiomatic code; if not possible, write comments.
 * Black-box testing (American) gives better bang-for-buck than white-box testing (UK).
 * If you’re reaching for the UI to perform an action and assert something is working, you should probably stop and write a test.
 * In white-box testing, only mock functions you own.
 * Prefer equality assertion over others (keep it simple).
 * Assert once against the whole output, instead of 10 times against various properties of the output.
 * Prefer asserting against hard-coded values.
 * Strike a balance between the unattainable ideal of one assertion per test and the fast-but-messy testing-everything-in-a-single-test.
 * Use a setup function instead of beforeEach. `beforeEach` is essentially inversion-of-control.
 * Leave the database messy after testing--i.e., perform clean up in set up function--so you can observe values in the case of a failure.
 * Divide tests into consistent sections. I recommend: set up, test code, assertions, and clean up.
 * Make sure your test assertions give good error messages.
 * Logging output in tests is an indication you need a better assertion.
 * Get as close to the parlance of the docs as possible—don’t put your own spin on it, unless the library goes against normal language conventions.
 * Composition over inheritance.
 * Composition over configuration.
 * The inversion-of-control pattern puts some framework developer in control instead of you. Avoid whenever possible.
 * Writing a few extra lines of code on the concrete level to gain the ultimate abstraction is worth it. AKA, Write Everything Twice, or Martin Fowler's "Rule of Three"
 * If you can’t deliver clean code on a deadline, it’s not the system’s fault; you’re probably just in over your head.
 * Keep the mainline pristine. Unused “maybe” code should be in a separate branch or behind a feature flag.
 * It’s a privilege to commit directly to mainline. >80% of your PRs should be flawless.
 * Thinking and planning before writing code saves time, as long as you’re painting with broad strokes.
 * Use git tags to denote any important commits/events, such as large code deletions.
 * Use guard clauses; conditionals should be one-level-deep max, almost all the time.
 * Handle errors first in functions.
 * PRs should not be optional, unless 80%+ of the PRs are “LGTM."
 * Use helper functions over global, mutable “context” variable.
 * Only and always write tests when: the cost of time spent doing manual tests > benefit of being able to change the architecture quickly, there’s a bug with code customers are currently using, there’s a bug with code that’s likely to exist for another year, or the consequences of a bug are catestrophic.
 * Don’t enforce a structure on Postgres jsonb column content. That’s better left to the actual schema.
 * Try to use not null with defaults so data is always returned with a consistent type.
 * Helper functions over higher-order functions over middleware over layers: https://github.com/zeit/micro/issues/8.
 * Mobile-first: only use min-width media queries.
 * Avoid ELK stack when possible. Really, be skeptical when the developer documentation is apologetic, especially if it looks pretty.
 * "The competent programmer is fully aware of the strictly limited size of his own skull; therefore he approaches the programming task in full humility, and among other things he avoids clever tricks like the plague." ~ Dijkstra.
 * "Just because you can do something, doesn't mean you should." ~ JL, responding to a ~200 line Postgres query.
 * The maximum size of redis values is 512MB. So, cache metrics in redis. Cache rows in postgres.
 * When dealing with dates, always check timezones.
 * Keep migrations small, to get better error messages, so they execute more quickly, and so you have a better idea of progress when making many changes.
 * Always create indicies on foreign keys.
 * Always create on delete (usually cascade) + on update triggers.


## DevOps

 * Every feature should be: automatically tested on every push, monitored (Prometheus or similar), and behind a feature flag.
 * Speed, speed, speed (in deployment).
 * Use feature flags so you can automatically push code to production, constantly.
 * When setting resource limits, remove all limits, add nodes, see new resource usage, and set new limits.
 * _2_ ounces of prevention (planning, protocols, restrictions, alerts) for every pound of potentially needed cure (the integral of [potential downside if things go] * [the likelihood that each will happen]).
 * When scaling, go expensive and messy. You can cut costs later, if it's even necessary.


## React

 * Divide components into three: pages, concrete, and abstract.
 * Follow barbell curve. abstract = proprietary UI library (basically same interface as other popular UI libraries), concrete = anything explicitly for your co as a use-case, pages = pages tied to HTTP routes. Aim for 0 concrete components.
 * Build abstract components whenever possible. Ideal distribution is 100% abstract and 100% pages, 0 concrete components.
 * Write readmes and react-docgen docs for abstract components, because they’re evergreen and open-sourceable.
 * Use HTML element names for components if possible.
 * Then use the WIMP names.
 * Then use names from other current UI libraries.
 * Then make up your own name, excluding common names that might confuse future developers.
 * Have consistent names for things from backend to frontend.


## Machine Learning

 * Just use Python (don’t get clever with javascript, C, R).
 * Use joblib.
 * Use sqlite if you can, for performance.
 * Set up schema, including foreign keys and not nulls, before importing data.
 * Start with the most fundamental table and work your way down.
 * Get the original data—don’t scrape summaries.
 * Use unique indices.
 * Just use a database, but only for simple queries.
 * If you need speed, create tables of cached values. CSV and filesystem are too clunky and rigid. Materialized views take too long to compute, can’t be parallelized/made more efficient, don’t have progress bars, are difficult to edit.
 * A good structure for any ML model is to define these functions: `scrape_data`, `cache_data`, `load_data`, `train`, `predict`, and `get_predictions`.
 * Make everything “lean” (aka, one-at-a-time), even when caching data. The simplicity and robustness is worth more than potential compute-time efficiency.
 * Put single-encoded values before one-hot encoded values for easier reading.
 * Get the original data, compute features if necessary.
 * Big data are generally more accurate than personalized small-dataset models.
 * Spot-check the data, especially when predictions look odd.
 * Deep learning out-performs xgboost with enough data.
 * Be _very_ wary of overfitting. Don’t train for more epochs than necessary.
 * Use small batches with deep learning, max 512.
 * Weight your samples.
 * If a human can’t look at the data and make an accurate prediction, a machine probably won’t be able to.
 * Encode between 0.1 and 0.9, to prevent any weights from zeroing out.
 * Don’t pass a huge amount of data through parameters, especially when using joblib. Use a cached “load_data” function instead.


## ETL/Scraping

 * Plan for failures in everything. Every service throws random 500s.
 * Be very clear what data is necessary, and what data you can default to null.
 * When resolving links between two services (same business, same person, etc), use any way of matching you can think of, but be very speicific and pessimistic about matches. For instance, use phone number, specific latitude + longitude (4 decimal places), exact same address.
 * Use queues with retries, and create 1 deployment per queue, with autoscaling on both the nodes and the pods.
 * If a scrape doesn't execute perfectly, log the reason why somewhere (like, in the database).
 * Call the service API directly, through puppeteer.
 * Source of truth should be implicit. Always allow updating data points from multiple sources.
 * Never use a massive, magic upsert. Inserts and updates are two different operations and should be handled explicitly.
 * Flow should be import -> verify/de-duplicate -> refresh.
 * Deduplication is a very important step and should be handled carefully. Decide upfront if you want manual deduplication (high quality, time consuming), or automatic deduplication (fast, but could introduce false positives/negatives).


## Management

 * One-on-ones every week are important for clearing the air.
 * Bring up disagreements/conflicts as soon as possible.
 * Give praise along with criticism. Bring up good with bad.
 * Communicate on the level of plans, instead of code or questions.
   * It’s easier to change a plan than concrete code or answer a million little questions.
 * Never press the self-destruct button.
 * Hanlon’s razor: “Never attribute to malice that which is adequately explained by stupidity.”
 * Use OKRs.
 * When writing OKRs, think of any big events you need to consider that’ll happen during the quarter.
 * When you deviate from OKRs, have a really, really good reason, or a really, really good question.
 * Everyone has a threshold for volatility, be aware of that and save it for important times.
 * Fire fast—everyone makes hiring mistakes (potentially a lot), and don’t let ego of good hiring prevent you from remedying the mistake. It’s not lack of mistakes, but how fast you adjust after noticing.
 * Give everyone their own kingdom.
 * OKRs are the guidepost, scrum is source of truth.
 * You can never escape planning, estimates, and time tracking. Just do them.
 * Poor planning on my part does not constitute an emergency on the people I manage or the people I report to.
 * Planning is as important as doing; it’s communication.
 * Planning can only speed you up.
 * Assumptions, clarified, instead of questions.
 * The time the process matters most is crunch time. Don’t panic; don’t press the “fuck it” button.
 * Laws can have privileges, but anyone should be able to get them.
 * Be wary of management debt you’ll need to pay back when raising money.
 * The quality of the company is determined by the quality of the people; you can only expand into greatness at the rate you can hire (or train) quality people.
 * People don’t rise to the occasion; they fall the level they’ve prepared for.
 * Quality is free, but you must pay dearly for it.
 * Tell people the rules, and why they exist.
 * Hold people accountable to consequences, not rule breaking. Let people get close to the customer.
 * “Let chaos reign, then reign in the chaos” - High Output Management
 * Double do instead of double check.
 * Write a good example before delegating.
 * When working with new employees, start tasks three weeks before they're due--1 week to complete the task, 1 week to review, and 1 week for you to re-do it for them if necessary.
 * Develop as if you're spending God's (or your own) money.


## Strategy

 * Identify profitable markets. Don’t build software for airlines.
 * Calculate the net present value when weighing choices.
 * Make many small, fast, high-risk bets with definite end points and definite results.
 * Bets/experiments/projects should build on one another.
 * Move as fast as possible, don’t get too invested in any one solution.
 * Quality follows a u-shape curve in relation to time--moving as fast as possible gives enormous bang-for-buck. Taking a medium amount of time is worst.


## Hiring

 * Don’t flake people in interview process, or lead people on in process.
 * If you have an interview, follow up.
 * Don’t re-neg on salary, be very thorough about salary.
 * Use trial periods, and be very explicit about them and their start/end dates.
 * Ask about books they've read.
 * Look for barbell curve--steeped in very old and very new technology.


## Sales

 * Always have a preorder form available.
 * Put together a “power statement.” (New Sales. Simplified.)
 * Divide time in thirds: 1/3 new sales, 1/3 pipeline, 1/3 maintaining current customers.
 * Arm your prospects with the tools they need to convince decision-makers.


## Marketing

 * Segment your market by psychographic traits (what they care about), rather than demographic traits.
 * Of your psychographic groups, clearly define the functional, monetary, psychological, and social benefits they care about.
 * Target your content at the people you want to reach.


## Other

 * Do all your work as if you're doing it for God or a higher power.
 * Always face the chaos. Don’t let the comfort of employment atrophy you.
 * Tell the truth, or least, if it's too difficult to discern what's true, don’t lie.
 * _Never_ walk away from a deal feeling like you’ve been “taken.”
 * Slow down negotiations, and don’t agree to anything verbally, prematurely.
 * “A foolish consistency is the hobgoblin of little minds.”
 * The best systems are emergent, organic, and antifragile.
 * Read every day.
 * Be wary of perfection fallacy—it does not have to be perfect to be valuable to someone.

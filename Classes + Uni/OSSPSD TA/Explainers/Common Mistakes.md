Interface related:
1. Having methods with actual code in your interface.
2. Interface is too specific.
	1. i.e.: Instead of a `chat_client_api`, making an interface called `slack_client_api`
	2. Interface is made such that it can only represent 1 service.
	3. Leaks of implementation details into interface. For example, defining an error like `TrelloNotFoundError` in an interface.
3. Using `@dataclass` or `Protocol` instead of ABC. (Not really a "mistake", but I've told them to go with ABC). This also includes using `Pydantic`. In your future, you can use whatever you want, but we're being extra specific in this class.

Implementation related:
1. Forgetting to actually implement the interface:
	1. For example, having a class e.g. `AIClientImpl` that does not implement the interface (doesn't do `AIClientImpl(AIClient)`)
2. Should do a pass about auth and why making `InstalledAppFlow` does not work.
3. Returning implementation specific exceptions instead of those defined in the interface.
4. Forgetting to use dependency injection.

Service related:
1. Should talk about the in-memory DB again here.
2. Forgetting to use dependency injection.

Adapter related:
1. Doesn't return the same types as those defined in an interface.
2. Forgetting to use dependency injection.
3. (very minor) no retry logic or idempotency.

Global:
1. Not using uv properly:
	1. Adding ruff.toml, mypy.ini, or other files instead of managing project in pyproject.toml
	2. Hatchling vs setuptools (not a big deal, I usually don't even mention it)
2. Not using ruff properly:
	1. Adding too many ignores
	2. Not adding `all`
3. Having files outside of the src folder of a component:
	Components should look something like:
		component/src/component/{code}
		component/tests/{code}
	Very rare, but teams sometimes forget to add code inside src.
4. PR-ing incorrectly:
	1. There are two types of PRs in this scenario: the ones you submit and the ones you use internally.
	2. The only PR we ever wanna see submitted for HW2 is from hw2 -> hw1 (or root if you already merged hw1 into root). This PR is unique and doesn't need to be replaced after every change in the hw2 branch.
	3. The other internal PRs from your feature branches into hw2 are there so you can squash big feats/units-of-work into hw2. These will update the main PR from before.
5. Ignoring cross-module type-checking in MyPy `[[tool.mypy.overrides]]`:
	1. Leads to MyPy not properly verifying if types work across modules and makes it somewhat useless in checking your work.

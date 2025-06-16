# Code Review

## Some personal thoughts

Ask some questions about the PR when we are reviewing the code:

- Finding and introducing the coding conventions and standard, so team could follow together (write some code style guide document for the team), (eg: Defined type `any` inside TypeScript codebase)

- Think from infrastructure level, is the code going to break agreed structure of existing codebase or would be harder to be maintained later on? (eg: Higher level maintainence of the existing codebase)

- Is the code satify with the business requirements? Or is it just a technical solution? [Don't forget to check the JIRA ticket descriptions and understand the business requirements]

- Is the code improved the tect debts or bring more to the exsiting codebase? (Eg: hardcode, same logic duplicated, etc ...)

- Is the code is wrong? (eg: code logic is not able to achieve the expected result etc ...)

- Is the code exposed some secrets, volunerabilities etc ...? (`Security` considerations)

- Please be very careful for the small and straight changes (We definitely are needed to think borader or wider when reviewing the small changes, sometimes, small changes could cause bigger issues)

- Is the code easy to be tested? (Unit test, integration test, etc ...) [Break the unit tests !!]

- When giving feedbacks, try to be more specific, and give some examples with a polite tone

- When receiving feedbacks, try to be more polite and ask constructive questions based on comments if certain parts are not clear

- Divide approval for 3 stages:

  - 1st stage: Merge blocker - the code better not merged into the `main/master/develop` branch

  - 2nd stage: To discuss - for the code we are not sure about, need to dive more

  - 3rd stage: Minor - Nit, optional, not important

- Better to have peer review for the PR, so we can get more feedbacks from the team before merge (slow down the merge process, specially when reviewing the PRs which are quite important or with previous tech debts, or bugfixes and so on)

- If the code is very important, we can try to set break points to check the code logic and the data flow (eg: use `debugger` statement in the code)

- Last but not least, lets always encourage team members in order to bring more positive energy to the team ~


Above are my code review thoughts, welcome to give me more feedbacks ~ Thanks for reading ~
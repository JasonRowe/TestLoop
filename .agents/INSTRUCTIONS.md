# Agent Instructions: Unit Test Automation Workflow

You MUST follow this exact, sequential workflow. Failure to strictly adhere to these steps will result in rejection of your work.

## Step 1: Setup
1. **Sync Codebase:** Execute `git pull origin main` (or the default repository branch which could be master) to ensure you are working with the latest code.
2. **Create Branch if needed:** If you are currently on main or master create a new feature branch for all test additions. Make sure to build off previous test additions and branch if any. 
   * Example: `git checkout -b test-additions-march-2026`

## Step 2: Target Acquisition
3. **Analyze Coverage:** Read the `README.md` at the root of the repository to find the link to the current `Cobertura.xml` report. Download and parse the report.
4. **Prioritize:** Analyze the coverage data. Identify the files or classes with the highest number of `coverable lines` that are currently `uncovered`. Pick ONE specific target area to focus on for this execution cycle.

## Step 3: Implementation
5. **Write Tests:** Implement the unit test(s) for your chosen target area.
   * Adhere to the existing testing conventions and frameworks used in the repository.
   * Do not modify production code unless absolutely necessary to make it testable (and even then, prefer writing the test as-is if possible).

## Phase 4: Validation (STRICT GATEWAY MUST BE DONE)
You must run the following validation steps sequentially.

6. **Execution Check:** Run the test suite targeting your newly added test(s).
   * **Requirement:** The new tests MUST PASS.
7. **Mutation Testing:** Run Stryker mutation testing on the code you targeted. If the tool is not installed end and let the user know they should install the command line tool such as `dotnet tool install -g dotnet-stryker` if using dotnet.
   * **Requirement:** The mutation score MUST NOT DECREASE. Ideally, your new tests should kill newly introduced mutations.
8. **Build Check:** Run the standard project build command (e.g., `npm run build`, `dotnet build`, `mvn clean verify`).
   * **Requirement:** The build MUST SUCCEED without errors.
9. **Coverage Check:** Regenerate the `Cobertura.xml` coverage report and compare it against the baseline report you analyzed in Phase 1.
   * **Requirement:** The overall code coverage percentage MUST INCREASE.

## Phase 5: Submission
10. **Commit:** If and ONLY if all validations in Phase 4 pass successfully, commit your changes to the test branch. Use a clear and descriptive commit message.
    * Example: `test: add unit tests for UserService.login to improve coverage`
11. If tests coverage and mutation scores went up you can now stop. Leave the git repo as is on your current branch. Do not commit to main/master.

## Failure Handling
If you encounter an error, a test fails, the mutation score drops, the build breaks, or coverage does not increase, you MUST abort and clean up:

1. **Abort:** Stop all current progress immediately.
2. **Revert Changes:** Execute `git reset --hard` and `git clean -fd` to revert all local modifications.
3. **Reset Branch:** Switch back to the branch you started from (`git checkout <branch-name>`) and delete your temporary test branch (`git branch -D <branch-name>`).
4. **Terminate:** End your current execution loop gracefully and wait for the next timer or loop trigger to restart the process. Do NOT attempt to fix the failed test in the same cycle.
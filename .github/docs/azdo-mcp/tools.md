🧿 Core
core_list_project_teams: Retrieve a list of teams for the specified Azure DevOps project.
core_list_projects: Retrieve a list of projects in your Azure DevOps organization.
⚒️ Work
work_list_team_iterations: Retrieve a list of iterations for a specific team in a project.
work_create_iterations: Create new iterations in a specified Azure DevOps project.
work_assign_iterations: Assign existing iterations to a specific team in a project.
📅 Work Items
wit_my_work_items: Retrieve a list of work items relevent to the authenticated user.
wit_list_backlogs: Revieve a list of backlogs for a given project and team.
wit_list_backlog_work_items: Retrieve a list of backlogs of for a given project, team, and backlog category.
wit_get_work_item: Get a single work item by ID.
wit_get_work_items_batch_by_ids: Retrieve list of work items by IDs in batch.
wit_update_work_item: Update a work item by ID with specified fields.
wit_create_work_item: Create a new work item in a specified project and work item type.
wit_list_work_item_comments: Retrieve list of comments for a work item by ID.
wit_get_work_items_for_iteration: Retrieve a list of work items for a specified iteration.
wit_add_work_item_comment: Add comment to a work item by ID.
wit_add_child_work_item: Create a child work item from a parent by ID.
wit_link_work_item_to_pull_request: Link a single work item to an existing pull request.
wit_get_work_item_type: Get a specific work item type.
wit_get_query: Get a query by its ID or path.
wit_get_query_results_by_id: Retrieve the results of a work item query given the query ID.
wit_update_work_items_batch: Update work items in batch.
wit_close_and_link_workitem_duplicates: Close duplicate work items by id.
wit_work_items_link: Link work items together in batch.
📁 Repositories
repo_list_repos_by_project: Retrieve a list of repositories for a given project.
repo_list_pull_requests_by_repo: Retrieve a list of pull requests for a given repository.
repo_list_pull_requests_by_project: Retrieve a list of pull requests for a given project Id or Name.
repo_list_branches_by_repo: Retrieve a list of branches for a given repository.
repo_list_my_branches_by_repo: Retrieve a list of my branches for a given repository Id.
repo_list_pull_request_threads: Retrieve a list of comment threads for a pull request.
repo_list_pull_request_thread_comments: Retrieve a list of comments in a pull request thread.
repo_get_repo_by_name_or_id: Get the repository by project and repository name or ID.
repo_get_branch_by_name: Get a branch by its name.
repo_get_pull_request_by_id: Get a pull request by its ID.
repo_create_pull_request: Create a new pull request.
repo_update_pull_request_status: Update status of an existing pull request to active or abandoned.
repo_reply_to_comment: Replies to a specific comment on a pull request.
repo_resolve_comment: Resolves a specific comment thread on a pull request.
🛰️ Builds
build_get_definitions: Retrieves a list of build definitions for a given project.
build_get_definition_revisions: Retrieves a list of revisions for a specific build definition.
build_get_builds: Retrieves a list of builds for a given project.
build_get_log: Retrieves the logs for a specific build.
build_get_log_by_id: Get a specific build log by log ID.
build_get_changes: Get the changes associated with a specific build.
build_run_build: Triggers a new build for a specified definition.
build_get_status: Fetches the status of a specific build.
🚀 Releases
release_get_definitions: Retrieves list of release definitions for a given project.
release_get_releases: Retrieves a list of releases for a given project.
🧪 Test Plans
testplan_create_test_plan: Creates a new test plan in the project.
testplan_create_test_case: Creates a new test case work item.
testplan_add_test_cases_to_suite: Adds existing test cases to a test suite.
testplan_list_test_plans: Retrieve a paginated list of test plans from an Azure DevOps project. Allows filtering for active plans and toggling detailed information.
testplan_list_test_cases: Gets a list of test cases in the test plan.
testplan_show_test_results_from_build_id: Gets a list of test results for a given project and build ID.
🔎 Search
search_code: Get the code search results for a given search text.
search_wiki: Get wiki search results for a given search text.
search_workitem: Get work item search results for a given search text.
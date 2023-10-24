# snowmate-bitbucket-runner
This is a Bash script for running Snowmate's tests in Bitbucket Pipeline.

## Add Snowmate's tests execution step in the pipeline
Add the Snowmate test run step in your project's `bitbucket-pipelines.yml` file:

```yml
image: python:3.10

pipelines:
  pull-requests:
    '**':
      - step:
          name: Run Snowmate tests
          script:
            - SNOWMATE_PROJECT_ID="" # Replace this with the project_id you have created on our website.
            - SNOWMATE_PROJECT_PATH="." # If this repo is not a monorepo, leave it as ".", otherwise, replace it with the relative path of the relevant service, e.g., "worker".
            - FEATURE_PROJECT_PATH=$(pwd)
            - TEMP_DIR=$(mktemp -d)

            - SNOWMATE_CI_SCRIPT_URL="https://github.com/snowmate/snowmate-bitbucket-runner.git"
            - BASH_SCRIPT_FOLDER="$TEMP_DIR/snowmate-bitbucket-runner"
            - BASH_SCRIPT_PATH="$BASH_SCRIPT_FOLDER/bitbucket-ci-runner.sh"

            ##### Fetching Snowmate's Bash script for running tests #####
            - git clone -b feature/effie/initial-commit $SNOWMATE_CI_SCRIPT_URL $BASH_SCRIPT_FOLDER
            - |
              chmod +x $BASH_SCRIPT_PATH
              "$BASH_SCRIPT_PATH" $SNOWMATE_PROJECT_ID $SNOWMATE_PROJECT_PATH $TEMP_DIR $FEATURE_PROJECT_PATH

```

For more information and installation, visit Snowmate's documentation [here](https://docs.snowmate.io/docs/33-test-execution-bitbucket-pipelines)

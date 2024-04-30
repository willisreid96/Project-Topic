# Project-Topic

Installation

(1) setup a virtual environment and run: pip install -r requirements.txt

(2) Duplicate the file alpha_codium/settings/.secrets_template.toml, rename it as .secrets.toml, and fill in your OpenAI API key:

[openai]
key = "..."
(3) Download the processed CodeContest validation and test dataset from hugging face, extract the zip file, and placed the extracted folder in the root of the project.

How to run
Configuration
The file: alpha_codium/settings/configuration.toml contains the configuration for the project. In the config section you can choose the model you want to use ("gpt-4", "gpt-3.5-turbo-16k", or others).

Solving a specific problem from CodeContest
To solve a specific problem with AlphaCodium, from the root folder run:

python -m alpha_codium.solve_problem \
--dataset_name /path/to/dataset \
--split_name test \
--problem_number 0
The dataset_name is the path to the dataset folder you downloaded in the installation step.
Note that the validation set contains 117 problems, and the test set contains 165 problems, so the problem_number parameter should be accordingly (zero-based)
The split_name can be either valid or test.
The following sections in the configuration file: solve, self_reflection,possible_solutions,generate_ai_tests,initial_code_generation,public_tests, ai_tests
enable to adjust possible configurations for the different stages of the flow.
Each run logs the results to a file named alpha_codium/example.log. Reviewing the log file is a good way to understand what is going on in each stage of the flow.
Solving an entire CodeContest dataset split
to solve the entire dataset with AlphaCodium, from the root folder run:

python -m alpha_codium.solve_dataset \
--dataset_name /path/to/dataset \
--split_name test
--database_solution_path /path/to/output/dir/dataset_output.json
The split_name can be either valid or test.
database_solution_path is the path to the directory where the solutions will be saved.
The dataset section in the configuration file contains the configuration for the running and evaluation of a dataset.
Note that this is a long process, and it may take a few days to complete with large models (e.g. GPT-4) and several iterations per problem.
dataset.num_iterations defines the number of iterations for each problem (pass@K). For a large number of iterations, it is recommended to introduce some randomness and different options for each iteration to achieve top results.
Running the evaluation
Once you generate a solution for the entire dataset (valid or test), you can evaluate it by running:

python -m alpha_codium.evaluate_dataset\
--dataset_name /path/to/dataset\
--split_name test\
--database_solution_path /path/to/output/dir/dataset_output.json
Solving a new problem (CodeContest format)
To solve a custom problem with AlphaCodium, first create a json file that includes the CodeContest problem fields, and then from the root folder run:

python -m alpha_codium.solve_my_problem \
--my_problem_json_file /path/to/my_problem.json
The my_problem_json_file is the path to to the custom problem json file.
See the my_problem_example.json to see an example of a custom problem. The json file should include the following fields:

name is the name of the problem.
description is a description of the problem.
(optional) public_tests with the following fields:
input is a list of strings that represent the input.
output is a list of strings that represent the output.
(optional) private_tests, that follows the same structure as public_tests
(optional) generated_tests, that follows the same structure as public_tests

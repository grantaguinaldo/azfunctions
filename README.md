### Azure Functions Workflow
- Create directory on desktop `az-func` and `cd` into that director. 
- Create a `conda.yml` file.
```
# conda.yml
name: az-func
dependencies:
	- pip
	- python=3.7
	- pylint
	- pip:
		- azure-functions
```
- Create a `venv` using the following `conda env create -f conda.yml`
- Activate virtual environment using `conda activate az-functions`
- Open VS code from command line using `code .`
- Once VS Code opens, go to Azure Icon on the bottom.
- Click the Add Functions icon to create a function. When promoted with box "The selected folder is not a function project. Create new project?" Click Yes.
- Inside of VS Code, to to Preferences > Extensions > Azure Functions and uncheck "Create a virtual environment when creating a new Python Project.
- Follow prompts in VS Code. We want to use `Python`, `Timer tigger`,  provide a name for the function `az-func-scrape`, enter the cron expression for the format, use `0 */1* * * *` so that we run every minute. Once you've selected everything, press enter and you'll create the project.
- Go to the `files` section in VS Code, and open the folder called `AZ_FUNCTOINS`. The `__init__.py` file is where the code that will be executed will be located and serves as the entry point of the function. `function.json` defines the bindings of the function. More inputs can be added as needed by the function.
- Log into the Azure portal and create a resource group with the same name as the folder that you have, in this case it's called `az-functions`. Have the region be `(US) West US 2`.
- Inside of the resource group that you created, next add an azure storage account by searching for `Storage account - blob, file, table, queue`. Create a storage account under your resource group with the name `gtaazfunctions`.
- Once the storage account has been created, click `Deploy to Function App`. In the popup window, click `Create new Function App in Azure ... (Advanced)`.
- Name the app `socalgas-ee-poc-scrape`, select runtime as `Python 3.7`, select `Consumption` hosting plan, select the resource group to deploy this function, in this case it's `az-functions`, then select storage account connected to this function, `gtaazfunctions` and lastly for the application insights, click `Skip for now`. Once all that has been selected, press enter and the function will deploy in Azure.

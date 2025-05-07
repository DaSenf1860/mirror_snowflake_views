# Mirror Snowflake Views to Microsoft Fabric

This repository provides a solution to mirror Snowflake views to Microsoft Fabric using Open Mirroring. It includes a set of scripts and configurations to facilitate the data transfer process.

The solution consists mainly of three notebooks:
- setup_resource.ipynb: This notebook sets up resources in Microsoft Fabric (Run this once for setting up a new mirroring solution)
- init_mirror_new_views.ipynb: This notebook will include new views to the mirroring routin (Run this once for every new view you want to mirror)
- sync_snowflake_views.ipynb: This notebook will sync the views from Snowflake to Microsoft Fabric (Run this continuously to keep the views in sync)



## Prerequisites

- Python 3.10 or higher
- Snowflake account with access to the source views and permission to create additional views and tables
- Microsoft Fabric account with contributor rights on the target workspace
- A service principal with contributor rights on the target workspace in Microsoft Fabric
- (optional) Azure Key Vault for storing credentials securely

## Set Up Instructions (approximately 5 minutes)

1. Download / clone this repository
2. Go to Microsoft Fabric ([app.fabric.com](https://app.fabric.microsoft.com/)). Navigate to the workspace where you want to set up the mirroring solution.
3. Import the notebooks from this repository into your Microsoft Fabric workspace.
4. Handle secrets:
4.1. (recommended) Go to the Key Vault and create six secrets for the Snowflake and service principal credentials. The secrets should be named as follows:
- snowflakeaccount
- snowflakeuser
- snowflakepassword
- mirroringspntenantid
- mirroringspnclientid
- mirroringspnclientsecret
4.2. (alternative) You can also set the credentials directly in the notebooks (init_mirror_new_views, sync_snowflake_views). Hardcode the values in the sections where "get_secret" and "getSecret" is used. This is not recommended as it exposes your credentials in the code. If you choose this option, make sure to remove the credentials from the code before sharing or deploying it.
5. Open the setup_resource.ipynb notebook:
5.1. Configure the first cell (after imports) if you want to use a specific workspace. If you set the workspace IDs to None it will use the workspace you are currently using.
5.2 Run the notebook. This will create an SQL Database and populate it with views. It also creates an Mirrored Database resource.
6. Set up an Environment item in Microsoft Fabric which uses the two public libraries from PyPI:
- snowflake-connector-python == 3.15.0
- msfabricpysdkcore ==  0.2.2
7. Open the init_mirror_new_views.ipynb notebook and attach the Environment item you created in step 6. This will allow the notebook to use the libraries from PyPI.
7.1 Configure the first cell (after imports) to initialize the mirroring for new views. You can also add new entries here (alternatively insert the values directly in the SQL Database).
7.2 Run the notebok. This will initialize the mirroring process for the views you want to mirror from Snowflake to Microsoft Fabric.
8. Open the sync_snowflake_views.ipynb notebook and run it. This will start the synchronization process, which will synchronize the views enabled for mirroring between Snowflake and Microsoft Fabric.
Repeat step 6 as often as you want to sync. You can also schedule this notebook to run at regular intervals using Microsoft Fabric's scheduling features.
# Step 3: Create a package that contains the required Python libraries<a name="tutorials-importing-data-create-python-package"></a>

The solution that's documented in this tutorial uses several libraries that aren't included with the standard Python package that Lambda uses\. To use these libraries, you first have to download them on your computer\. Next, you create a `.zip` archive that contains all of the libraries\. Finally, you upload this archive in Lambda so that you can call the libraries from your functions\.

**Note**  
This procedure assumes that you've already installed Python\. Python is included by default with most recent Linux, macOS, or Unix distributions\. If you use Windows, you can download Python from the [Python releases for Windows page](https://www.python.org/downloads/windows/) on the Python website\. This solution was tested using Python version 3\.7\.3 on macOS High Sierra, Ubuntu 18\.04 LTS, and Windows Server 2019\.

**To download the libraries that are required for this tutorial**

1. At the command line, enter the following command to install the `virtualenv` package:

   ```
   python -m pip install virtualenv
   ```

1. Enter the following command to create a new directory for the virtual environment:

------
#### [ Linux, macOS, or Unix ]

   ```
   mkdir ~/pinpoint-importer
   ```

------
#### [ Windows PowerShell ]

   ```
   new-item pinpoint-importer -itemtype directory
   ```

------

1. Enter the following command to change to the `pinpoint-importer` directory:

   ```
   cd pinpoint-importer
   ```

1. Enter the following command to create and initialize a virtual environment called `venv`:

   ```
   python -m virtualenv venv
   ```

1. Enter the following command to activate the virtual environment: 

------
#### [ Linux, macOS, or Unix ]

   ```
   source venv/bin/activate
   ```

------
#### [ Windows PowerShell ]

   ```
   .\venv\Scripts\activate
   ```

------

   Your command prompt changes to show that the virtual environment is active\.

1. Enter the following command to install the packages that are required for this tutorial:

   ```
   pip install s3fs jmespath s3transfer six python-dateutil docutils
   ```

1. Enter the following command to deactivate the virtual environment:

   ```
   deactivate
   ```

1. Enter the following command to create an archive that contains the necessary libraries:

------
#### [ Linux, macOS, or Unix ]

   ```
   cd venv/lib/python3.7/site-packages && \
   zip -r ~/pinpoint-importer/pinpoint-importer.zip dateutil docutils jmespath \
   s3fs s3transfer fsspec six.py && cd -
   ```

   In the preceding command, replace *3\.7* with the version of Python that's installed on your computer\.

------
#### [ Windows PowerShell ]

   ```
   cd .\venv\Lib\site-packages\
   Compress-Archive -Path dateutil, docutils, jmespath, s3fs, s3transfer, six.py `
   -DestinationPath ..\..\..\pinpoint-importer.zip ; cd ..\..\..
   ```

------

**Next**: [Create the Lambda Function That Splits Input Data](tutorials-importing-data-lambda-function-input-split.md)
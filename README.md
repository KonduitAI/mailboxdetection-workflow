Workflows for Mailbox Detection
-----------------------------------
This workflow illustrates the integration of UiPath and Konduit-Serving with the example of mailbox detection. 
This workflow able to detect the location of each mailbox and whether it's empty or not.  
![MailBox Result](img/mailboxbeforeafter.PNG "MailBox Result")

File Descriptions
----------------------
**Main.xaml**: The main entry point. This contains the whole workflow for starting a server.

**ScoreImage.xaml**: For use after the server is started. This allows you to send an image specified as a file path to 
the server for detection. This is meant to be more of an example that should be integrated in to a larger workflow.

**DownloadAndExtract.xaml**: This automatically downloads and extracts a server where the
mail box detection model will run

**ConfigureServer.xaml**: Handles configuring the server configuration (a json file) to point to your current directory
where you are running the workflow. This is necessary to start the server it (happens automatically)

Intended usage
------------------------------------
### Step 1: 
Run Main.xaml first. This will download a server distribution from the releases
of this repository containing everything needed to run the model.

After this, ensure the server output has "INFO: Deployed verticle {}" in the text.

![Started Server](img/startedserver.png "Started Server")


If there are any exceptions, please file a github issue.

### Step 2:
To test it, submit a test image using ScoreImage.xaml.  
This is a workflow that contains an http request that handles calling the server via some configured parameters.

The only thing that needs to be changed for your own custom image is the image path variable in the workflow.

After this, a series of alerts will display indicating the output from the model and will also display an
image that was created that displays the labels found in the image.

Integration usage
------------------------------

The json output from the model should be used and integrated in to a larger workflow
that takes action on whether there is mail present in the submitted picture.

 A sample workflow named ScoreImage.xaml is bundled
 with the server to show how to invoke the server.

 You pass in an input image path and you can get output from the server. 

 ![Use Score Image](img/usescoreimage.png "Score image")

Afterwards, the workflow will output the coordinates and labels that are to be used in an external integration:

 ![AI Output](img/aioutput.png "AI Output")



Troubleshooting
--------------------------------

If the download fails, follow the following steps:

1. Download the [zip file](https://github.com/KonduitAI/mailboxdetection-workflow/releases/download/v1.1/distro.zip)
containing the model and server. Extract it to *mailboxdetection-workflow* directory, which is the directory where your workflows are.

2. Run Main.xaml. This will invoke ConfigureServer.xml after checking that the zip file exists. The ConfigureServer.xml configure the server to start using the proper directory. This is required to start the server.
   ![Replace Picture](img/replacepicture.png "Replace Picture")
   ![Replace Picture Result](img/replacepictureresult.png "Replace Picture Result")

   The successful running of Main.xaml will start the server. 

![Started Server](img/startedserver.png "Started Server")



If the ScoreImage.xaml fails to run the first time as intended above, this maybe due to a request time out due to the server
needing to startup the first time. Just rerun the workflow
and you should see the intended results.

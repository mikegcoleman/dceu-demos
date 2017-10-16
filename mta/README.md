# MTA

You can find videos of all show demos [here](https://drive.google.com/drive/folders/0ByQd4O58ibOENWtWT0xUakxGa0U?usp=sharing)

This demo is designed to give a very simplistic example of an MTA process.

We have a VHD in our Windows VM, and we're going to use Image2 Docker to extract that VM. From there we build the docker image, and then create the service up on UCP. 

## Demo Setup

1. RDP into the Windows host (windows.dockerdemos.com) username `mta` password has been provided. 

2. Check to see the Windows tweet image has been built and pushed to DTR. If it has not run through the demo once to prebuild it and prepush it.

3. Log into ucp (ucp.dockerdemos.com) and dtr (dtr.dockerdemos.com). Username `mta` password has been provided

4. Make sure the Windows tweet app service is not running on UCP

## Create the dockerfile with Image2Docker


1. Open up a powershell window

3. Use Image2Docker's `ConvertTo-Dockerfile` command to create a dockerfile from the VHD.

	```
	ConvertTo-Dockerfile -ImagePath c:\ws2016.vhd -Artifact IIS -OutputPath C:\windowstweetapp -Verbose
	```
	
When the process completes you'll find a dockerfile in `c:\windowstweetapp`

## Build and Push Your Image to Docker Trusted Registry

1. CD into the `c:\windowstweetapp` directory (this is where your Image2Docker files have been placed).

2. Use `docker build` to build your Windows tweet web app Docker image.

	`$ docker build -t dtr.dockerdemos.com/mta/windows_tweet_app .`

4. Log into Docker Trusted Registry

	```
	PS C:\> docker login dtr.dockerdemos.com
	Username: mta
	Password: <password>
	Login Succeeded
	```

5. Push your new image up to Docker Trusted Registry.

	```
	docker push dtr.dockerdemos.com/mta/windows_tweet_app
	```

### <a name="task3.3"></a> Task 3.3: Deploy the Windows Web App
Now that we have our Windows Tweet App up on the DTR server, let's deploy it. 

1. Switch  to UCP in your web browser

2. In the left hand menu click `Services`

3. In the upper right corner click `Create Service`

4. Enter `windows_tweet_app` for the name.

4. Under `Image` enter the path to your image which should be `dtr.dockerdemos.com/mta/windows_tweet_app`

8. From the left hand menu click `Network`

8. Set the `ENDPOINT SPEC` to `DNS Round Robin`. 

9. Click `Publish Port+` Fill out the port fields as Follows:

    * `Internal Port`: 80
    * `Publish Mode`: Host 
    * `Public Port`: 8082

PLEASE make sure you use the correct por

11. Click 'Confirm'

12. Click `Create` near the bottom right of the screen.

After a few seconds you should see a green dot next to your service name. 

Once you see you green dot you can  point your web browser to `http://windows.dockerdemo.com:8082` to see your running website.

## Clean Up after each Demo

1. Delete the `windows_tweet_app` service in UCP

2. In the Windows VM remove the `c:\windowstweetapp` directory

## Windows VHD


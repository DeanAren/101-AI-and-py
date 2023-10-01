## Instructions to create the application for AI Deploy

Here you find all steps to create and run an image to create an application based on the previous trained the model (with AI Training).

### Image build

The Python script need to access to the following path `/workspace/attendee`.

This location is mounted as volumes of the image, the paths for the host is the OVHcloud object container storage used in the Notebook: `attendee-$STUDENT_ID-data`.

The registry used to store the image is the an Harbor registry provided by OVHcloud, ask the speakers for the URL and account.

In `/src/app/`, build the image using Docker: `docker build . -t $REGISTRY_NAME/$STUDENT_ID/yolov8-rock-paper-scissors-app:1.0.0`.  
Then, authenticate to the registry: `docker login $REGISTRY_NAME  -u $REGISTRY_LOGIN -p $REGISTRY_PASSWORD`.
⚠️ Perhaps you have to remove some previous created Docker images to free space to build this image ⚠️
Then, push the builded image: `docker push $REGISTRY_NAME/$STUDENT_ID/yolov8-rock-paper-scissors-app:1.0.0`.

### App creation

 - run the app with AI App:

```bash
ovhai app run \
	--name attendee-$STUDENT_ID-yolov8-rock-paper-scissors-app \
	--cpu 1 \
	--default-http-port 8501 \
	--volume attendee-$STUDENT_ID-data@GRA:/workspace/attendee:RW:cache \
	--unsecure-http \
	$REGISTRY_NAME/$STUDENT_ID/yolov8-rock-paper-scissors-app:1.0.0
```
- get the logs: `ovhai app logs -f <app id>`
- get the URL:
	- get the application id: `ovhai app list` 
	- find the field `Url` in the details og the application with the command: `ovhai app get <app id>`
	

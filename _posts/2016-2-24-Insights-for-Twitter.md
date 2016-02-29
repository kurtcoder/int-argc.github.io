---
layout: post
title: Insights for Twitter
permalink: /insights-for-twitter/
---

The Insights for Twitter service allows us to query Twitter to incorporate Twitter Content to our applications.

Download Insights for Twitter Powerpoint Presentation [here](https://github.com/kurtcoder/twitterinsightsresources/blob/master/Insights-For-Twitter-Ley.pptx?raw=true)


In this tutorial you will deploy a sample Insights for Twitter Application in Bluemix. You will also deploy the application using the Devops Delivery Pipeline, and You will also familiarize yourself with the service.

#### Fork a Github Repository
You will fork the repository that you will deploy using the Devops Delivery Pipeline.

1. Go to [GitHub](https://github.com) and Login.

2. Go to the Github Repository [https://github.com/kurtcoder/insights-for-twitter](https://github.com/kurtcoder/insights-for-twitter)

3. Click the `Fork` button to make your own copy of the repository.

<br>

#### Create a Bluemix DevOps Project based on the Github Repository 

1. Open another tab in your web browser and login to [Bluemix DevOps] (https://hub.jazz.net)

2. Click  `Create Project` 
	
3. Name your project `insights-for-twitter`.
	
4. Click `Link to an existing GitHub repository`.  

5. Select the repository `<username>/insights-for-twitter`

6. Ensure the following options are chosen:

	||||
	|---|---|---|
	| **Private Project** | checked |
	| **Add features for Scrum development** | checked |
	| **Make this a Bluemix Project** | checked |
	| **Region** | IBM Bluemix US South |
	| **Organization** | you may leave the default selection |		
	| **Space** | dev |

	<br>

7. Click the `CREATE` button and wait for your project to be created.

#### Create a Build Stage

1. Click the `BUILD & DEPLOY` button.

2. Click the `Add Stage` button. 

3. Set the name from `My Stage` to `Build Stage`

4. On the `Input` Tab, set the following values

	||||
	|---|---|---|
	| **Input Type** | SCM Repository |
	| **Git URL** | https://github.com/<username>/insights-for-twitter.git |
	| **Branch** | master |
	| **Stage Trigger** | Run jobs whenever a change is pushed to Git |

	<br>

5. Go to the `Jobs` Tab and click `Add Job` and set `Build` as the Job type.

6. Rename `Build` to `Gradle Assemble`

7. Set the following values

	||||
	|---|---|---|
	| **Builder Type** | Gradle |		
	| **Build Shell Command** | `#!/bin/bash`<br>`gradle assemble`  |	
	| **Stop running this stage if this job fails** | checked |

	<br>

8. Click the `Save` Button.	

#### Create a Deploy Stage

1. Click the `Add Stage` Button

2. Rename `MyStage` to `Deploy Stage`

3. On the `Input` Tab, set the following values

	||||
	|---|---|---|
	| **Input Type** | Build Artifacts |
	| **Stage** | Build Stage |
	| **Job** | Gradle Assemble |
	| **Stage Trigger** | Run jobs when the previous stage is completed |

	<br>
	
4. On the `Jobs` Tab Click `Add Job` and select `Deploy`

5. Rename `Deploy` to `Cloud Foundry Push to Dev Space`.

6. Set the following values

	||||
	|---|---|---|
	| **Deployer Type** | Cloud Foundry |		
	| **Target** | IBM Bluemix US South - https://api.ng.bluemix.net |		
	| **Organization** | you may leave the default selection |		
	| **Space** | dev |	
	| **Application Name** | blank |		
	| **Deploy Script** | `#!/bin/bash`<br> `cf push insights-for-twitter-<your_name> -m 256M -p build/libs/twitterinsights.war` |	
	| **Stop running this stage if this job fails** | checked |

	<br>

7. Click `Save`

You have created a delivery pipeline.

#### Deploy The application

1. Click the `Run Stage` icon of the `Build Stage`

	>Make sure all the stages pass before proceeding to the next step.

2. Go to the [Bluemix] (https://console.ng.bluemix.net) Website and Login.

3. Go to the `Dashboard` Tab and click the widget of your application `insights-for-twitter-<yourname>`

4. Click `Add a Service or API`

5. In the Catalog page look for the `Insights for Twitter` service and click it.

6. Rename the Service name to `Insights for Twitter - <yourname>` . Select the `Free Plan` Option in the `Selected Plan` Dropdown.

7. Click `Create`

8.  When prompted to Restage Application, press `Restage`

You have successfully deployed your Insights for Twitter Application

#### Using the Application

1. On a new tab, access your new application using the url `http://insights-for-twitter-<yourname>.mybluemix.net`
2. The first operation that the web application can do is to count the number of tweets, given a keyword
	> e.g. #ibm, binay

The Result for the #ibm query will look like this
	>> {"related":{"search":{"href":"https://cdeservice.mybluemix.net:443/api/v1/messages/search?q=%23ibm"}},"search":{"results":198618}} 

1. Look for the `Text to Speech` service and click it.

1. In the `Service name` text box, type `Text to Speech - <your-name>`.

1. Click the `CREATE` button.

1. When asked to restage your application, click the `RESTAGE` button.  Wait for your application to restage.

1. Open another browser tab (do not close the browser tab containing your Bluemix account).  Go to `http://t2s-<your_name>.mybluemix.net` to test if the sample application can already connect to the Text to Speech service.

1. In the text box, type any word or sentence that you want to synthesize. (For this tutorial, we can only use English words and sentences, but the service can synthesize Brazilian Portuguese, English, French, German, Italian, Japanese, and Spanish)
	
1. Click the `Submit` button.  
2. After clicking the submit button, a file will be downloaded automatically.
3. Locate the downloaded file and rename it. Append a `.wav` at the end of the file name.
4. Play the wav file using any audio player.

	<br>

####Analyze how the Text to Speech Application works

The code shownbelow is the source code found in the Text to Speech Servlet:


	```
		
		// Part 1
			TexttoSpeechConnector connector = new TexttoSpeechConnector();      
  			TextToSpeech service = new TextToSpeech();
  			service.setUsernameAndPassword(connector.getUsername(),connector.getPassword());
        	// Part 2
        		String text = request.getParameter("inputText");
        		String format = "audio/wav";

  			InputStream speech = service.synthesize(text, format);
  		// Part 3
            		OutputStream output = response.getOutputStream();

			    byte[] buf = new byte[2046];
				int len;
				while ((len = speech.read(buf)) > 0) {
					output.write(buf, 0, len);
				}
                        
                response.setContentType("audio/wav");
    
				//ServletContext ctx = getServletContext();  
				response.setHeader("Content-disposition","attachment;filename=output.wav");  
 
				OutputStream os =output;   
                                
				os.flush();  
				os.close();  
	```
	Part 1 shows the connection to the service. The credentials are obtained and are passed to the service.
	
	Part 2 shows the input to output process. In the first part, the text is retrieved from the index.jsp and the format is set to a wav file. After which, it is passed to the service with the use of the service.synthesize method. The parameters needed are the text and the file format, which are both strings. The method returns an InputStream. The synthesize method can be used using the "import com.ibm.watson.developer_cloud.text_to_speech.v1.TextToSpeech;" import.
	
	Part 3 shows the download process. An OutputStream is created and the InputStream is written there. The OutputStream is then flushed in order for the file to be downloaded.
	
	Note: It is important to remember that the service can output ogg, flac, and wav files. The one shown above uses a wav file as the output. In order to make use of the application for other file types the "format" string must be adjusted accordingly.

####Delete the sample application for housekeeping

1. Go to Bluemix.
2. Click Dashboard.
3. From the Applications List, click the gear icon of the created application. In the Services tab, make sure that the Text to Speech service is selected. In the Routes tab, make sure that the route (i.e., URL) is selected.
4. Choose Delete App.

<br>

####End of Tutorial


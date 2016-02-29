---
layout: post
title: Insights for Twitter
permalink: /insights-for-twitter/
---

The Insights for Twitter service allows us to query Twitter to incorporate Twitter Content to our applications.

Download Insights for Twitter Powerpoint Presentation [here](https://github.com/kurtcoder/insights-for-twitter/blob/master/Insights-For-Twitter-Ley.pptx?raw=true)


In this tutorial you will deploy a sample Insights for Twitter Application in Bluemix. You will also deploy the application using the Devops Delivery Pipeline, and You will also familiarize yourself with the service.

#### Fork a Github Repository
You will fork the repository that you will deploy using the Devops Delivery Pipeline.

1. Go to [GitHub](https://github.com) and Login.

2. Go to the Github Repository [https://github.com/kurtcoder/insights-for-twitter/tree/master/InsightsForTwitterApp](https://github.com/kurtcoder/insights-for-twitter/tree/master/InsightsForTwitterApp)

3. Click the `Fork` button to make your own copy of the repository.

<br>

#### Create a Bluemix DevOps Project based on the Github Repository 

1. Open another tab in your web browser and login to [Bluemix DevOps] (https://hub.jazz.net)

2. Login to your Bluemix account using the `cf` tool.

	```text
	> cf login -a https://api.ng.bluemix.net -s dev
	```
	
	>When asked for a username (e-mail address) and password, enter the username and password of your Bluemix account.
	
	>-a to specify the API endpoint (https://api.ng.bluemix.net).
	
	>-s to specify space name (dev).

	<br>
	
3. Upload the sample application to your Bluemix account.

	```text
	> cf push t2s-<your_name> -m 256M -p t2s.war
	```

	**Example:**
		
	```text
	> cf push t2s-coloma -m 256M -p t2s.war
	```
	-m to specify the memory to be allocated for the application being pushed (256M)
	
	-p to specify the file to be uploaded (t2s.war)
	
	-<your-name> is for you to have a unique bluemix url

	<br>
	
1. Go to IBM Bluemix Website and Login .  Once logged in, click `DASHBOARD`.  

	The `Applications` section of your dashboard shows a widget representing the application `t2s-<your_name>` you deployed earlier.

	
	<br>
	
1. Click the widget of your application to see its overview.
	
1. Click the `ADD A SERVICE OR API` link.  You will be redirected to the `Catalog` page. 

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


package com.Whatsapp.Api;


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

@SpringBootApplication
public class WhatsappApiApplication {

	public static void main(String[] args) {
		  // Define the phone number and phone number ID
		// Start the Spring application
	    SpringApplication.run(WhatsappApiApplication.class, args);
	    
		  String phoneNumber = "254702647717";
		  String phoneNumberId = "102368222669265";
		  String authToken = "EAAQZAPtwjyZAgBAG2AuZBlP9RaOecu9J34XMjVEmsd4U4Wt1CiJjX9unyh8MWIszvS70ujghfJmk2G0ZCnfjGTyGoR4ClZC2BCZB214HJIgPuDdZAveFGTPAEqileoZCIXfyQeEm0sCdOTYcnUe4cVBceDzYPbgeIIqGUx40DqF70BNDKaY7wBFs";

		  try {
		    // Create the JSON body for the POST request
		    String jsonBody = "{ \"messaging_product\": \"whatsapp\", \"recipient_type\": \"individual\", \"to\": \""+phoneNumber+"\", \"type\": \"text\", \"text\": { \"preview_url\": false, \"body\": \"This is an example of a text message\" } }";

		    // Build the POST request
		    HttpRequest postRequest = HttpRequest.newBuilder()
		                                     .uri(URI.create("https://graph.facebook.com/v15.0/" + phoneNumberId + "/messages"))
		                                     .header("Authorization", "Bearer "+authToken)
		                                     .header("Content-Type", "application/json")
		                                     .POST(HttpRequest.BodyPublishers.ofString(jsonBody))
		                                     .build();

		    // Build the GET request
//		    HttpRequest getRequest = HttpRequest.newBuilder()
//		                                    .uri(URI.create("https://graph.facebook.com/v15.0/" + phoneNumberId + "/messages"))
//		                                    .GET()
//		                                    .header("Authorization", "Bearer "+authToken)
//		                                    .build();
		    
		    // Send the HTTP requests
		    HttpResponse<String> postResponse = HttpClient.newHttpClient().send(postRequest, HttpResponse.BodyHandlers.ofString());
//		    HttpResponse<String> getResponse = HttpClient.newHttpClient().send(getRequest, HttpResponse.BodyHandlers.ofString());

		    // Print the response status code and body
		    System.out.println("POST response status code: "+postResponse.statusCode());
		    System.out.println("POST response body: "+postResponse.body());
//		    System.out.println("GET response status code: "+getResponse.statusCode());
//		    System.out.println("GET response body: "+getResponse.body());

		  } catch (IOException | InterruptedException e) {
	            e.printStackTrace();
	        }
		}

}

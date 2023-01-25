package com.Whatsapp.Api;

import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
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

	public static void main(String[] args) throws IOException, InterruptedException, URISyntaxException {
		// Define the phone number and phone number ID
		String phoneNumber = "254702647717";
		String phoneNumberId = "102368222669265";
		String authToken = "EAAQZAPtwjyZAgBAG2AuZBlP9RaOecu9J34XMjVEmsd4U4Wt1CiJjX9unyh8MWIszvS70ujghfJmk2G0ZCnfjGTyGoR4ClZC2BCZB214HJIgPuDdZAveFGTPAEqileoZCIXfyQeEm0sCdOTYcnUe4cVBceDzYPbgeIIqGUx40DqF70BNDKaY7wBFs";

		// Start the Spring application
		SpringApplication.run(WhatsappApiApplication.class, args);

		HttpClient httpClient = HttpClient.newHttpClient();

		while (true) {
			// Send a request to get the list of received messages
			HttpRequest request = HttpRequest.newBuilder()
					.uri(new URI("https://graph.facebook.com/v15.0/" + phoneNumberId + "/messages"))
					.header("Authorization", "Bearer " + authToken).build();
			HttpResponse<String> response = httpClient.send(request, HttpResponse.BodyHandlers.ofString());

			// Parse the JSON response to get the list of messages
			ObjectMapper objectMapper = new ObjectMapper();
			JsonNode root = objectMapper.readTree(response.body());
			JsonNode messages = root.path("messages");

			// Iterate through the messages and log them
			for (JsonNode message : messages) {
				String body = message.path("body").asText();
				System.out.println("Received message: " + body);

				// Send an automated response to the sender
//				String sender = message.path("author").asText();
				request = HttpRequest.newBuilder().uri(new URI("https://graph.facebook.com/v15.0/" + phoneNumberId + "/messages"))
						.header("Authorization", "Bearer " + authToken).header("Content-Type", "application/json")
						.POST(HttpRequest.BodyPublishers.ofString("{\"to\":\"" + phoneNumber
								+ "\",\"body\":\"Thank you for your message. I am an automated response and am not able to respond to individual messages. If you have any further questions, please feel free to contact us through our website.\"}"))
						.build();
				httpClient.send(request, HttpResponse.BodyHandlers.ofString());
			}

			// Wait for a minute before checking for new messages again
			Thread.sleep(60000);
		}
	}
}

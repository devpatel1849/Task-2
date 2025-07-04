# Task-2
REST API Client

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

import org.json.JSONObject;

public class WeatherClient {

    public static void main(String[] args) {
        String apiKey = "your_api_key"; // 🔁 Replace with your actual API key
        String city = "Ahmedabad";
        String urlString = "https://api.openweathermap.org/data/2.5/weather?q=" + city + "&appid=" + apiKey + "&units=metric";

        try {
            // Step 1: Create URL and open connection
            URL url = new URL(urlString);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();

            // Step 2: Set request method
            conn.setRequestMethod("GET");

            // Step 3: Get the response code
            int responseCode = conn.getResponseCode();
            if (responseCode == 200) { // OK
                BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
                String inputLine;
                StringBuilder response = new StringBuilder();

                while ((inputLine = in.readLine()) != null) {
                    response.append(inputLine);
                }
                in.close();

                // Step 4: Parse JSON Response
                JSONObject json = new JSONObject(response.toString());
                String cityName = json.getString("name");
                JSONObject main = json.getJSONObject("main");
                double temperature = main.getDouble("temp");
                int humidity = main.getInt("humidity");

                // Step 5: Display Output
                System.out.println("City: " + cityName);
                System.out.println("Temperature: " + temperature + " °C");
                System.out.println("Humidity: " + humidity + " %");
            } else {
                System.out.println("HTTP GET Request Failed with Error Code: " + responseCode);
            }

        } catch (Exception e) {
            System.out.println("Error occurred: " + e.getMessage());
        }
    }
}


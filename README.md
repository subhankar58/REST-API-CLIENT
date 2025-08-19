# REST-API-CLIENT

*COMPANY*: CODTECH IT SOLUTIONS

*NAME*: SUBHANKAR PALAI

*INTERN ID*: CT06DH1831

*DOMAIN*: FRONT END DEVELOPMENT

*DURATION*: 4WEEKS

*MENTOR*: NEELA SANTHOSH

## DESCRIPTION ##

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;
import org.json.JSONObject;

public class WeatherApp {

    private static final String API_KEY = "28bd1c6d39443ad70c426073e90d7523";
    private static final String BASE_URL =
            "https://api.openweathermap.org/data/2.5/weather?q=%s&units=metric&appid=%s";

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter city name: ");
        String city = scanner.nextLine().trim();

        try {
            String jsonResponse = getWeatherData(city);
            displayWeather(jsonResponse);
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    private static String getWeatherData(String city) throws Exception {
        String urlString = String.format(BASE_URL, city, API_KEY);
        URL url = new URL(urlString);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");

        int status = conn.getResponseCode();
        BufferedReader reader;

        if (status == 200) {
            reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        } else {
            reader = new BufferedReader(new InputStreamReader(conn.getErrorStream()));
        }

        String inputLine;
        StringBuilder response = new StringBuilder();

        while ((inputLine = reader.readLine()) != null) {
            response.append(inputLine);
        }
        reader.close();

        if (status != 200) {
            throw new Exception("API Error: " + new JSONObject(response.toString()).optString("message", "Unknown error"));
        }

        return response.toString();
    }

    private static void displayWeather(String json) {
        JSONObject obj = new JSONObject(json);

        String cityName = obj.getString("name");
        JSONObject main = obj.getJSONObject("main");
        double temp = main.getDouble("temp");
        double feelsLike = main.getDouble("feels_like");
        int humidity = main.getInt("humidity");

        JSONObject weather = obj.getJSONArray("weather").getJSONObject(0);
        String description = weather.getString("description");

        System.out.println("\n=== Weather Info ===");
        System.out.println("City: " + cityName);
        System.out.println("Temperature: " + temp + "°C");
        System.out.println("Feels Like: " + feelsLike + "°C");
        System.out.println("Humidity: " + humidity + "%");
        System.out.println("Description: " + description);
    }
}


#OUTPUT

<img width="741" height="293" alt="Image" src="https://github.com/user-attachments/assets/89e21d29-e644-4be8-90ec-48437a07a839" />

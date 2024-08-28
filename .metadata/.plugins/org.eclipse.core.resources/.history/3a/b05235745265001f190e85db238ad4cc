package com.app;

import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import java.io.FileReader;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Map;
import java.util.Random;

public class MD5HashApp {
    public static void main(String[] args) {
        if (args.length < 2) {
            System.out.println("Usage: java -jar JsonMD5Hash.jar 240341220203 \"F:\\Bajaj_Health_Test\\JsonMD5Hash\"");
            return;
        }

        String prnNumber = args[0].toLowerCase().trim();
        String jsonFilePath = args[1];

        try {
            String destinationValue = getDestinationValueFromJson(jsonFilePath);
            if (destinationValue == null) {
                System.out.println("Key 'destination' not found in the JSON file.");
                return;
            }

            String randomString = generateRandomString(8);

            String concatenatedValue = prnNumber + destinationValue + randomString;

            String md5Hash = generateMD5Hash(concatenatedValue);

            System.out.println(md5Hash + ";" + randomString);

        } catch (Exception e) {
            System.out.println("An error occurred: " + e.getMessage());
        }
    }

    private static String getDestinationValueFromJson(String jsonFilePath) throws Exception {
        JsonElement jsonElement = JsonParser.parseReader(new FileReader(jsonFilePath));
        return findDestinationKey(jsonElement);
    }

    private static String findDestinationKey(JsonElement element) {
        if (element.isJsonObject()) {
            JsonObject jsonObject = element.getAsJsonObject();
            for (Map.Entry<String, JsonElement> entry : jsonObject.entrySet()) {
                if ("destination".equals(entry.getKey())) {
                    return entry.getValue().getAsString();
                }
                String value = findDestinationKey(entry.getValue());
                if (value != null) {
                    return value;
                }
            }
        } else if (element.isJsonArray()) {
            for (JsonElement arrayElement : element.getAsJsonArray()) {
                String value = findDestinationKey(arrayElement);
                if (value != null) {
                    return value;
                }
            }
        }
        return null;
    }

    private static String generateRandomString(int length) {
        String characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
        Random random = new Random();
        StringBuilder sb = new StringBuilder(length);
        for (int i = 0; i < length; i++) {
            sb.append(characters.charAt(random.nextInt(characters.length())));
        }
        return sb.toString();
    }

    private static String generateMD5Hash(String input) throws NoSuchAlgorithmException {
        MessageDigest md = MessageDigest.getInstance("MD5");
        byte[] hashBytes = md.digest(input.getBytes());
        StringBuilder sb = new StringBuilder();
        for (byte b : hashBytes) {
            sb.append(String.format("%02x", b));
        }
        return sb.toString();
    }
}


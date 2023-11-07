# Link-shortener
import java.util.HashMap;
import java.util.Map;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public class LinkShortener {
    private Map<String, String> urlShortenerMap;
    private static final String BASE_URL = "http://short.ly/";
    
    public LinkShortener() {
        urlShortenerMap = new HashMap<>();
    }

    public static void main(String[] args) {
        LinkShortener linkShortener = new LinkShortener();

        String longURL = "https://www.example.com";
        String shortURL = linkShortener.shortenURL(longURL);

        System.out.println("Long URL: " + longURL);
        System.out.println("Short URL: " + shortURL);

        String originalURL = linkShortener.expandURL(shortURL);
        System.out.println("Original URL: " + originalURL);
    }

    public String shortenURL(String longURL) {
        String hash = generateHash(longURL);
        String shortURL = BASE_URL + hash;
        urlShortenerMap.put(shortURL, longURL);
        return shortURL;
    }

    public String expandURL(String shortURL) {
        if (urlShortenerMap.containsKey(shortURL)) {
            return urlShortenerMap.get(shortURL);
        } else {
            return "Short URL not found";
        }
    }

    private String generateHash(String input) {
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");
            byte[] digest = md.digest(input.getBytes());
            StringBuilder hash = new StringBuilder();

            for (byte b : digest) {
                hash.append(String.format("%02x", b));
            }

            return hash.toString();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
            return "";
        }
    }
}

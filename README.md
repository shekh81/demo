import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import java.io.IOException;

public class BestBuyAutomation {

    public static void main(String[] args) throws IOException {

        // Navigate to Best Buy website
        String url = "https://www.bestbuy.com/";
        Document doc = Jsoup.connect(url).get();

        // Pull data from HTML table
        Element dealOfTheDayLink = doc.select(".h-text-md:contains(Deal of the Day)").first();
        String dealOfTheDayUrl = dealOfTheDayLink.attr("href");
        Document dealOfTheDayDoc = Jsoup.connect(dealOfTheDayUrl).get();
        Elements tableRows = dealOfTheDayDoc.select("table > tbody > tr");
        for (Element row : tableRows) {
            System.out.println(row.text());
        }

        // Pull data from DIV in HTML
        Element tvsLink = doc.select("#menu0 > a").first();
        String tvsUrl = tvsLink.attr("href");
        Document tvsDoc = Jsoup.connect(tvsUrl).get();
        Element productContainer = tvsDoc.select(".row-xpod").first();
        String productName = productContainer.select(".sku-header > h4").text();
        String productPriceString = productContainer.select(".priceView-customer-price > span").text();
        double productPrice = Double.parseDouble(productPriceString.replaceAll("[^0-9.]+", ""));
        System.out.println(productName + " - " + productPrice);

        // Demonstrate data scraping and string manipulation
        Element headphonesLink = doc.select("#menu0 > a:contains(Headphones)").first();
        String headphonesUrl = headphonesLink.attr("href");
        Document headphonesDoc = Jsoup.connect(headphonesUrl).get();
        Element headphonesContainer = headphonesDoc.select(".row-xpod").first();
        String productName2 = headphonesContainer.select(".sku-header > h4").text();
        String productPriceString2 = headphonesContainer.select(".priceView-customer-price > span").text();
        double productPrice2 = Double.parseDouble(productPriceString2.replaceAll("[^0-9.]+", ""));
        System.out.println(productName2 + " - " + productPrice2);
        Element reviewLink = headphonesContainer.select(".ugc-reviews-main-link:contains(Write a review)").first();
        String reviewUrl = reviewLink.attr("href");
        Document reviewDoc = Jsoup.connect(reviewUrl).get();
        String reviewTitle = reviewDoc.select(".ugc-review-main-body > h2").text();
        System.out.println(reviewTitle);

        // Interact with a modal window
        Element cartLink = doc.select("#header-cart").first();
        String cartUrl = cartLink.attr("href");
        Document cartDoc = Jsoup.connect(cartUrl).get();
        Element productContainer2 = cartDoc.select(".cart-item-grid").first();
        Element addToCartButton = productContainer2.select(".btn-primary:contains(Add to Cart)").first();
        String addToCartUrl = addToCartButton.attr("href");
        Document addToCartDoc = Jsoup.connect(addToCartUrl).get();
        Element checkoutButton = addToCartDoc.select(".btn-secondary:contains(Checkout)").first();
        String checkoutUrl = checkoutButton.attr("href");
        Document checkoutDoc = Jsoup.connect(checkoutUrl).get();
        Element firstNameInput = checkoutDoc.select("#consolidatedAddresses.ui-address-card-fields > div >

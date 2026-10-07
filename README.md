Emergeo Belle WEBSITE: SETUP GUIDE
==============================

What's in this folder
---------------------
index.html     Home page
shop.html      Shop all wigs (category pills, sort, search results)
product.html   Single wig page (gallery with roll-to-zoom, length/colour/cap options, reviews)
cart.html      Cart with promo codes
wishlist.html  Saved wigs
signup.html    Create account, sign in, and "My account" with order history
payment.html   Checkout: contact, address, delivery method, card / bank transfer / pay on delivery
Code.gs        The Google Apps Script that connects the website to your Google Sheet

Each HTML page carries its own CSS and JavaScript inside it, so there are no extra files to upload.
Open index.html in a browser right now and the whole site works with sample content.


Step 1: Create the Google Sheet
-------------------------------
1. Create a new blank Google Sheet and name it "Emergeo Belle Website".
2. Click Extensions > Apps Script.
3. Delete what's in Code.gs, then paste in everything from the Code.gs file in this folder. Save.
4. In the function dropdown at the top, choose "setup" and click Run.
5. Google will ask for permission. Click Review permissions, pick your account, then Advanced > Go to project > Allow.
6. Go back to the Sheet. You'll now have 10 tabs filled with sample content, and a "Emergeo Belle" menu at the top.


Step 2: Publish the script
--------------------------
1. In Apps Script, click Deploy > New deployment.
2. Click the gear next to "Select type" and choose Web app.
3. Execute as: Me. Who has access: Anyone.
4. Click Deploy and copy the Web app URL (it starts with https://script.google.com/macros/s/...).


Step 3: Connect the pages
-------------------------
Open each of the 7 HTML files in a text editor (Notepad, VS Code) and find this line near the top of the <script> section:

    const API_URL = "PASTE_YOUR_WEB_APP_URL_HERE";

Replace PASTE_YOUR_WEB_APP_URL_HERE with your Web app URL. In VS Code you can do all 7 files at once with Find and Replace in Files (Ctrl+Shift+H).


Step 4: Put the site online
---------------------------
Upload the 7 HTML files to any host (GitHub Pages, Netlify, cPanel). Code.gs and this guide don't need uploading.


Changing content from the Sheet
-------------------------------
Store-wide sale: set store_discount_percent in Settings (e.g. 10). It applies to every wig that has no
discount_percent of its own. Set it back to 0 to end the sale. round_discount_prices = TRUE keeps prices
as whole numbers.

Settings     Shop name, logo, announcement bar, hero text and image, offer banner, currency, shipping fees,
             payment options, bank details, contact details, social links. Column C explains each row.
             Leave a value blank to use the default. Type a single - to hide something.
Menu         Every link in the top bar, main menu and footer. "position" decides where it shows:
             top, main, footer_shop or footer_help.
Categories   The round "Shop by category" pictures on the home page.
Highlights   All the small icon rows: hero_strip (under the hero), trust_bar (home), product_trust
             (product page), cart_trust (cart page) and signup_perks (sign-up page).
Products     One row per wig. See the product notes below.
Reviews      Reviews customers send from the product page land here with approved = FALSE.
             Change it to TRUE to show the review on the site.
Promos       Promo codes. type is percent or fixed, value is the amount, min_subtotal is optional,
             expires is an optional date. Set active to FALSE to switch a code off.
Customers    Sign-ups. Passwords are stored as salted hashes, never as plain text. Don't edit salt, hash or token.
Orders       Every order with items, totals, payment method and status. Update order_status yourself
             (for example New, Packed, Shipped, Delivered). Customers see it in My Account.
Newsletter   Emails from the "Join now" offer box and from sign-ups that ticked the newsletter box.

The website keeps a copy of the Sheet for up to 5 minutes so it loads fast. To see changes straight away,
use Emergeo Belle > Refresh website now in the Sheet, then reload the page with ?refresh=1 at the end of the address
(for example index.html?refresh=1).


Images, logo and icons
----------------------
Anywhere you see a grey silhouette, there's an image slot waiting for a link.
- Logo: paste a link in Settings > logo_url. With logo_mode set to image+text it sits beside the shop name.
  Set logo_mode to image-only if your logo already includes the name.
- Hero, offer and sign-up pictures: hero_image, offer_image and signup_image in Settings.
- Category pictures: image column in Categories.
- Product pictures: image is the main photo. gallery takes more links separated by commas.
- Icons: every row in Highlights has an icon_url column. Paste a PNG or SVG link there to replace the
  built-in line icon. Built-in icon names you can type in the icon column: truck, shield, lock, returns,
  headset, hair, lace, head, leaf, heart, tag, bag, card, bank, cash, bolt, chat, info, comb, check, mail, phone, pin.

Google Drive images work too. Right-click the file > Share > General access: Anyone with the link,
then paste the normal share link. The site converts it automatically.


Product columns
---------------
id              Unique code, e.g. LL012. Don't change it once a wig has been ordered.
category        One or more categories separated by |  e.g. Lace Front|Body Wave
badge           Text on the pill, e.g. Best Seller, New, Sale, Limited. Leave blank for none.
badge_color     Optional colour code for the badge, e.g. #c9a24a
price           Normal price before any discount.
discount_percent  Type 20 for 20% off. The site works out the new price itself and shows the old price
                crossed out next to it with a -20% tag, on the home page, shop, product page and cart.
                Customers are charged the discounted price. Leave blank for no discount.
compare_price   Only if you'd rather type the old price yourself instead of using discount_percent.
length_prices   Optional different price per length, e.g. 16:209,18:229,20:249,22:289
default_length  Length selected when the page opens. Blank picks the middle one.
colors          Name:colour code, separated by |  e.g. Natural Black:#111111|Honey Blonde:#e9dbbf
lengths         Separated by commas, e.g. 16,18,20,22,24
cap_sizes       Separated by commas, e.g. Small,Medium,Large
details         Spec lines for the Description tab, separated by |  e.g. Lace: 13x6 HD lace|Density: 180%
how_to_wear, care   Press Ctrl+Enter inside the cell to start a new line.
featured        TRUE shows the wig in "Featured products" on the home page.
active          FALSE hides the wig from the site.
stock           0 shows "Sold out". 1 to 5 shows "Only X left". Blank means not tracked.
sort            Lower numbers show first.

Room for 360 wigs: in the Sheet, click Emergeo Belle > Add product slots (up to 360). It adds empty rows with
IDs already filled in (LL012, LL013 ... LL360). A row only appears on the site once it has a name and price,
so you can fill them in at your own pace. The shop shows 24 wigs per page with page numbers at the bottom.
Change products_per_page in Settings if you want more or fewer per page.


Payments (Paystack)
-------------------
1. Create a Paystack account and copy your keys from Settings > API Keys & Webhooks.
2. Public key (pk_test_ or pk_live_) goes in the Sheet: Settings > paystack_public_key.
3. Secret key (sk_test_ or sk_live_) never goes in the Sheet. In Apps Script click Project Settings (gear icon)
   > Script properties > Add script property. Name: PAYSTACK_SECRET_KEY. Value: your secret key. Save.
4. Set currency_code to a currency your Paystack account accepts. Nigerian accounts use NGN by default,
   so if you price in dollars you'll need USD enabled on Paystack, or switch the site to naira
   (currency_symbol ₦, currency_code NGN, and update product prices).

How it works: the order is saved first with payment_status "Awaiting payment". After the customer pays,
the script checks the payment directly with Paystack and changes the status to "Paid" only if the amount
and currency match. Prices are always re-calculated from the Sheet, so nobody can change a price in their browser.

Bank transfer and pay on delivery need no setup apart from bank_details and cod_note in Settings.
Switch any method off with pay_card_enabled, pay_bank_enabled or pay_cod_enabled = FALSE.


Order emails
------------
Put your email in Settings > notify_email to get an email for every new order and every confirmed payment.
Customers get an order email automatically (and a receipt when card payment is confirmed).
Google allows about 100 emails a day on a free Gmail account.


If you edit Code.gs later
-------------------------
Go to Deploy > Manage deployments > pencil icon > Version: New version > Deploy. This keeps the same URL,
so you won't need to change the HTML files again.


Preview mode
------------
Until you paste the Web app URL, the site runs on sample data. Sign-up, checkout and promo codes still work
so you can click through everything, but nothing is saved. The test promo code in preview mode is WELCOME20.

# Unofficial Guide to Google Analytics Cookie Banners

Disclaimer: I am not a lawyer so do not take this as official legal advice. This is my best-effort interpretation of the resources below.

This is an unofficial set of guidelines to use of Google Analytics with cookie banners / notices, synthesized from the following resources:

- https://blog.oriel.io/2019/01/24/how-to-make-google-analytics-gdpr-compliant/
- https://webdevlaw.uk/wp-content/uploads/2018/05/WordCamp-Belfast-GDPR-Google-Analytics.pdf
- https://brianclifton.com/blog/2018/04/16/google-analytics-gdpr-and-consent/
- https://brianclifton.com/blog/2018/05/21/gdpr-request-consent-before-tracking/
- https://www.blastam.com/blog/gdpr-need-consent-for-google-analytics-tracking

Twitter discussion: https://twitter.com/karlhorky/status/1126025690443329536

## Step 1: Usage before Consent

Google Analytics can be used without users giving consent by clicking on an accept button, if configured correctly:

> If you use \[the] Advertising features in GA, you must request explicit consent. If you do not, then you don’t.

### Step 1a: Set the following Google Analytics GDPR settings

1. If you haven't yet, read and accept the *Data Processing Amendment* under `Admin` -> `Account Settings` [Ref](https://blog.oriel.io/2019/01/24/how-to-make-google-analytics-gdpr-compliant/)
2. Uncheck all `Data Sharing Settings` checkboxes under `Admin` -> `Account Settings` [Ref](https://blog.oriel.io/2019/01/24/how-to-make-google-analytics-gdpr-compliant/)
3. For each property:
   1. Make sure *Enable Demographics and Interest Reports* is **off** under `Admin` -> `Property Settings` [Ref](https://webdevlaw.uk/wp-content/uploads/2018/05/WordCamp-Belfast-GDPR-Google-Analytics.pdf)
   2. Make sure both *Remarketing* and *Advertising Reporting Features* are **off** under `Admin` -> `Tracking Info` -> `Data Collection` [Ref](https://blog.oriel.io/2019/01/24/how-to-make-google-analytics-gdpr-compliant/)
   3. Make sure the User-ID feature is **off** under `Admin` -> `Tracking Info` -> `User-ID` [Ref](https://blog.oriel.io/2019/01/24/how-to-make-google-analytics-gdpr-compliant/)
4. Make sure that you never track pages with personal information in them (query parameters, for example) [Ref](https://webdevlaw.uk/wp-content/uploads/2018/05/WordCamp-Belfast-GDPR-Google-Analytics.pdf)

### Step 1b: Anonymize the IP

On creation of the tracker, disable the following features ([Ref](https://www.blastam.com/blog/gdpr-need-consent-for-google-analytics-tracking)):
   ```js
   ga('create', 'UA-XXXXX-Y', {
     // Make IP addresses anonymous, reducing accuracy
     anonymizeIp: true,
     // Only required if you will continue with Step 2 and re-enable any settings above
     allowAdFeatures: false,
   });
   ```

### Step 1c: Privacy Policy section   

Mention Google Analytics in your privacy policy with instructions how to remove cookies.

## Step 2: Usage after Consent

If your strategy requires advertising features, you may enable them programmatically by calling the `set` method after consent. Any necessary features will need to be enabled again in the property (Step 1a point 3).

1. Design a prominent cookie notice that users will notice, to improve engagement with it
2. Once the user accepts the cookies, you may enable following features again ([Ref](https://www.blastam.com/blog/gdpr-need-consent-for-google-analytics-tracking)):
   ```js
   if (userAccepted) {
     ga('set', {
       allowAdFeatures: true,
       anonymizeIp: false,
     });
   }
   ```

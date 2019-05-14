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

Google Analytics can be used without users giving consent (consent example: clicking on an accept button), if configured correctly:

> If you use \[the] Advertising features in GA, you must request explicit consent. If you do not, then you don’t.

### Step 1a: Set the following Google Analytics GDPR settings

These settings assume that you will not need the advertising features.

1. If you haven't yet, read and accept the *Data Processing Amendment* under `Admin` -> `Account Settings` [Ref](https://blog.oriel.io/2019/01/24/how-to-make-google-analytics-gdpr-compliant/)
2. Uncheck all *Data Sharing Settings* checkboxes under `Admin` -> `Account Settings` [Ref](https://blog.oriel.io/2019/01/24/how-to-make-google-analytics-gdpr-compliant/)
3. For each property, disable the advertising features you don't need. If you do need them, leave them on and make sure to implement [Step 1b Option 2](#step-1b-option-2-anonymize-the-ip-and-disable-all-advertising-features) and [Step 2](#step-2-usage-after-consent-optional---for-advertising-features):
   1. Make sure *Enable Demographics and Interest Reports* is **off** under `Admin` -> `Property Settings` [Ref](https://webdevlaw.uk/wp-content/uploads/2018/05/WordCamp-Belfast-GDPR-Google-Analytics.pdf)
   2. Make sure both *Remarketing* and *Advertising Reporting Features* are **off** under `Admin` -> `Tracking Info` -> `Data Collection` [Ref](https://blog.oriel.io/2019/01/24/how-to-make-google-analytics-gdpr-compliant/)
   3. Make sure the *User-ID* feature is **off** under `Admin` -> `Tracking Info` -> `User-ID` [Ref](https://blog.oriel.io/2019/01/24/how-to-make-google-analytics-gdpr-compliant/)
4. Make sure that you never track URLs with personal information in them (query parameters, for example) [Ref](https://webdevlaw.uk/wp-content/uploads/2018/05/WordCamp-Belfast-GDPR-Google-Analytics.pdf)

### Step 1b Option 1: Anonymize the IP

If you do not need any advertising features, on creation of the tracker, set `anonymizeIp` to `true` ([Ref](https://www.blastam.com/blog/gdpr-need-consent-for-google-analytics-tracking)):

```js
ga('create', 'UA-XXXXX-Y', {
  // Make IP addresses anonymous, reducing accuracy
  anonymizeIp: true,
});
```

### Step 1b Option 2: Anonymize the IP and disable all advertising features 

Alternatively, if you need advertising features, you can disable them until you get consent by setting `allowAdFeatures` to `false` ([Ref](https://www.blastam.com/blog/gdpr-need-consent-for-google-analytics-tracking)):

```js
ga('create', 'UA-XXXXX-Y', {
  // Make IP addresses anonymous, reducing accuracy
  anonymizeIp: true,
  // Disable any default-enabled Advertising features (turn them on later when we get consent)
  allowAdFeatures: false,
});
```

### Step 1c: Privacy Policy section   

Mention Google Analytics in your privacy policy with instructions how to remove cookies (opt-out).

## Step 2: Usage after Consent (optional - for advertising features)

If your strategy requires advertising features such as *Demographics and Interest Reports*, *Remarketing with Google Analytics* and *DCM Integration*, you need to  enable them programmatically by calling the `set` method after consent. Any necessary features will need to be enabled again in the property (Step 1a point 3).

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

## References

### Non-Google Analytics Tracking without Consent

- [VS Code Telemetry](https://twitter.com/freiksenet/status/1128264608236675072)

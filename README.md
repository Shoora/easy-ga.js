# easy-ga.js
Easy to Set-up Google Analytics Tracking Supporting Enhanced Ecommerce Transactions (EC) &amp; Respecting Data Privacy User Choices (Do-not-track Browser Settings &amp; Opt-out Cookies)

Docs: https://www.robert-matthees.de/ecommerce/easy-ga/

easy-ga.js - Easy to Implement JavaScript Plugin (Open Source)
Inspired by the General Data Protection Regulation (GDPR) & the European ePrivacy Regulation (ePR), I was looking into a Google Analytics Set-up that respects the Data Privacy Choices of Website Visitors / Online Shop Users, but still tracks their Orders & Transactions properly. The outcome is a little script:

Checking the User's Privacy Choices with regard to Opt-out Cookies & Do-not-track Browser-Settings prior to the Initialisation of any Google Analytics Tracking Code
Providing an Opt-out by Default Option where Users are required to Opt-in before any Tracking takes place
Starting (or not!) Google Analytics Tracking accordingly to the User's Settings via analytics.js with anonymizeIp=true
Adding the right Functions to your Google Analytics Opt-out-Link & Opt-in Link as they need to be a part of your Cookie Policy / Privacy Agreement (including confirmation messages supporting a nice UX)
Not Tracking any Onsite Behaviour of Users with Do-not-Track Settings / Cookie Opt-out or Users who Disables Cookies in their Browser if the Opt-out by Default Mode is active, but still tracks their Orders / Transactions in Google Analytics by an Ajax Call to a little PHP-Script that forwards the Enhanced Ecommerce Transaction (EC) via Google's Measurement Protocol (Google Analytics Backend Tracking)
Detecting Users with strict AdBlocker Rules that kicked out your Google Analytics JavaScript Tracker + Track their Transactions via Ajax & Measurement Protocol too
Display Customizable Warning Messages to Users whose Privacy Choices prevent you from Tracking explaining that optimizing their on-site experience isn't possible because of their current settings (+ further targeting scenarios)
Important: There are Do-not-track Settings activated in your Browser which prevent us from optimizing your experience on our site.

Get easy-ga.js on GitHub (version: 0.1)

Set-up Google Analytics Tracking
Example-Code:

var myTracker = new Tracking({
  'property': 'UA-8238XXXX-1' //***Your Google Analytics ID
});

Init Default Pageview
Example-Code:

myTracker.init(); //***Send Pageview

Add a Google Analytics Tracking Opt-out / Opt-in Link to your Privacy Agreement / Cookie Policy
Example-Link: Google Analytics Tracking: opt-out

Example-Code:

<a href="javascript:myTracker.opt()">Google Analytics Tracking: opt-<span>out</span><span>/</span><span>in</span></a>
Info: The first Span-Element contains the message shown when the User is able to perform an Opt-out (creating the Opt-out Cookie), the last Span-Element contains the message for an Opt-in (deleting an existing Opt-out Cookie). The one in the middle can be used as separator for an initial, pre-rendered view.

Another Example without a Span-Element in the middle: opt-out

Example-Code:

<a href="javascript:myTracker.opt()"><span>opt-out</span><span>opt-in</span></a>
Same as:

<a href="javascript:myTracker.opt()">opt-<span>out</span><span>in</span></a>

Track a Purchase Transaction with Google Analytics Enhanced Ecommerce (EC)
Example-Data:

//***Test Data for Purchase Transaction Tracking

//*1. Array with 1 to n Product Objects:
var products = [{
  'id': '12333',
  'name': 'mini skirt ',
  'category': 'woman clothing', //***optional
  'brand': 'best skirts', //***optional
  'variant': 'black', //***optional
  'price': '30',
  'quantity': 5
},{
  'id': '12334',
  'name': 'striped socks',
  'category': 'man clothing',
  'brand': 'hardly new',
  'variant': 'red dot pink black',
  'price': '1',
  'coupon': 'OLDMANSALE', //***optional | Coupon Code (Product Level)
  'quantity': 1
}];

//*2. Transaction Object:
var transaction = {
  'id': 'abc1',
  'affiliation': 'CookieDropShop', //***optional | name of affiliate partner responsible for transaction
  'revenue': '155.99', //***optional | either total or without tax/shipping (up to you)
  'tax': '24.91', //***optional
  'shipping': '4.99', //***optinal
  'coupon': 'SUMMERSALE1' //***optional | Coupon Code (Order Level)
};
Example-Code:

myTracker.init({ 'track': {
  'action': 'purchase',
  'products': products, //***Your Array with Product Obejects
  'transaction': transaction //***Your Transaction Object
}});

Available Tracker Options
Separated by comma:

'property': 'UA-8238XXXX-1', //***Enter Your Google Analytics ID | string | no default

'php': 'easy-ga.php', //***Where is the PHP Script located? | string | default: easy-ga.php

'selector': 'a[href$=".opt()"]', //***CSS Selector of Opt-out Link | string | default: a[href$=".opt()"]

'default_optout': 1, //***Tracking Switched Off by Default (just track after opt-in) | bool | default: 0

'opt_link': 1, //***Change Text of Opt-Out/In Links | bool | default: 1

'msg_confirm': 1, //***Show Opt-out/in Confirmation on Link Interaction? | bool | default: 1

//***Confirmation Messages | string | defaults below
'msg_txt_opt_in': 'opt-in successfull (tracking active)',
'msg_txt_opt_out': 'opt-out successfull (tracking stopped)',
'msg_txt_conflict': 'cookie opt-in, but -do not track- browser settings (still no tracking)',
'msg_txt_no_cookie': 'you need to enable cookies in your browser settings to use this feature',
'msg_txt_no_cookie_optout': 'you need to enable cookies in your browser settings to use this feature (tracking disabled by default)',

//***Confirmation Message Colors | string | defaults below
'msg_cl_opt_in': '#009400',
'msg_cl_opt_out': '#009400',
'msg_cl_conflict': '#ff0000',
'msg_cl_no_cookie': '#ff0000',
'msg_cl_no_cookie_optout': '#ff0000',

'msg_time': 2750, //***How long should the confimation message be displayed? (doubles when conflict / no cookie message) | integer in ms | default: 2750

'msg_container': '.msg-ga', //***Confirmation Msg in a specific Container instead of Link replacement? | string | empty on default

'warning': 1, //***Show Warning Message | bool | default: 0

//***Warning Messages | string | defaults below
'warning_optout': 'Important: There is an Opt-out Cookie saved in your Browser that prevents us from optimizing your experience on our site.',
'warning_do_not_track': 'Important: There are Do-not-track Settings activated in your Browser which prevent us from optimizing your experience on our site.',
'warning_both': 'Important: There is an Opt-out Cookie saved in your Browser & Do-not-track Settings which prevent us from optimizing your experience on our site.',
'warning_adblock': 'Important: Using AdBlockers is like Stealing Pocket Money from Children in Primary School.', //***Not 100% accurate: The Warning Message just comes up when an AdBlock User is not using Do-not-track Settings or an Opt-out Cookie at the same time as otherwise no Goggle Analytics will be started in the Frontend at all (and its running Functionality is used to detect AdBlockers)
'warning_cookies_disabled': 'Please enable Cookies in your Browser Settings to enjoy all features on our site.',

'warning_container': '.warn-ga', //***Where to display the warning message? CSS Selector | string | empty on default

'debug': 1 //***Debug Mode for Console Log Output | bool | default: 0

To Do
Further Enhanceed Ecommerce Features
UID/ClientID Support

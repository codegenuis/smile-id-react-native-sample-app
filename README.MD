

## Get Started

1. To install the SDK run the below command in the terminal at the root of your project, run
npm install rn-smile-id
2. The Smile ID Config file (smile_config.json) would be shared with you or downloaded from the smile id portal
3. For iOS open the ios project and place this file at the root of your project and for android place this file at the root of the main module (usually app folder) this step is critical and your project will behave unexpectedly if these are not done right.

## Android Setup
1. In your root-level (project-level) Gradle file (build.gradle), add rules to include the Maven Central repository.
<br/>
    buildscript {​    
       &nbsp;&nbsp;&nbsp;&nbsp; repositories {        
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; mavenCentral() // add the maven central repository <br/>
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; maven { <br>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url 'https://oss.sonatype.org/content/repositories/snapshots/' <br>
       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}

2. In your module (app-level) Gradle file (usually app/build.gradle), apply the Smile ID android plugin:
    <br/>
    apply plugin: 'com.smileidentity.smile-id-android'

3. Declare the Smile ID android SDK in your root level Gradle file (usually build.gradle).
<br/>
dependencies {  <br/>
   &nbsp;&nbsp;&nbsp;&nbsp; classpath group: 'com.smileidentity', name: 'smile-id-android', version: '1.0.1' <br/>
    &nbsp;&nbsp;&nbsp;&nbsp;}

4. Add the following permissions if they do not exist already
<br/>

    `<uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />  
    <uses-permission android:name="android.permission.INTERNET" />  
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />  
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />  
    <uses-permission android:name="android.permission.READ_PHONE_NUMBERS" />`

5. Add the following activities under the application tag
<br/>

`<activity android:name="com.smileid.smileidui.SIDIDCaptureActivity" />
<activity android:name="com.smileid.smileidui.SIDSelfieCaptureActivity" />
<activity android:name="com.smileid.smileidui.SIDReviewActivity" />`

## ios setup
The SDK only runs on 64 bit processors arm64 and x86_64 and does not support armv7 and i386. 

### Cocoapods
Close your XCode if your project is open and from the terminal, navigate to the iOS folder in the react native and run 
<br /> 
pod install 


<br/><br/>
## Selfie And ID Capture
<h3> Import the Library </h3>
import RnSmileId from 'rn-smile-id';

<h3> Capture Selfie </h3>

1. If you want the SDK to generate a tag for you then call the method with an empty string
<br />
    const data = await RnSmileId.captureSelfie('');

2. If you want the generate to use a custom tag send a string to the SDK with alphanumeric characters  with the exception of an underscore and no spacing in them.
<br/>
    const data = await RnSmileId.captureSelfie(<STRING_TAG>);

<h3>Get Results </h3>
 The selfie capture process is an asynchronous process and will return an error code and a tag once done. The result looks as the below

 const resultCode = data.SID_RESULT_CODE;<br/>
 const resultTag = data.SID_RESULT_TAG;

 If the result code is -1 AND the tag is not null or empty then the selfie capture was successful.


<br/><br/>
 ## Submit Job 
  const result = await RnSmileId.submitJob(
      <br/>
               &nbsp;&nbsp;&nbsp;&nbsp; tag: string,
                <br/>
               &nbsp;&nbsp;&nbsp;&nbsp; jobType: number,
                <br/>
               &nbsp;&nbsp;&nbsp;&nbsp; isProduction: boolean,
                <br/>
               &nbsp;&nbsp;&nbsp;&nbsp; partnerParams: object,//optional pass {} when not available
                <br/>
                &nbsp;&nbsp;&nbsp;&nbsp; idInfo: object,//optional pass {} when not available
                <br/>
               &nbsp;&nbsp;&nbsp;&nbsp; geoInfo: object//optional pass {} when not available
               <br/>
              );

<ul>
    <li>tag: required and if this is job type which involve a selfie it must have been a tag that has already captured a job before this method is called.</li>
    <li>jobType: required and must be a valid job type as listed on the products page on the left </li>
    <li>isProduction: required and is a boolean true or false. If false the jobs will be sent to sandbox if true jobs will be sent to production</li>
    <li>partnerParams: {
        "job_id": "unique id",
        "user_id": "unique id",
    }
    </li>
    <li>idInfo:  (optional) This is mandatory for document verification but is used as the case may be for job type Enhanced KYC + SmartSelfie. the object is described below <br/>
    {
        <br/>
        &nbsp;&nbsp;&nbsp;&nbsp; country: "country",<br/>
        &nbsp;&nbsp;&nbsp;&nbsp;id_type: "idType",<br/>
        &nbsp;&nbsp;&nbsp;&nbsp;id_number: "idNumber",<br/>
        &nbsp;&nbsp;&nbsp;&nbsp;first_name: "firstName",<br/>
        &nbsp;&nbsp;&nbsp;&nbsp;middle_name: "middleName",<br/>
        &nbsp;&nbsp;&nbsp;&nbsp;last_name: "lastName",<br/>
        &nbsp;&nbsp;&nbsp;&nbsp;email: "email",<br/>
    }
    </li>
</ul>

## Get Results

The method will return a result accessible in our example as result.result. <br>
This object is an object documented in the return value section of your chosen product type under the products section on the left the most important of these being the  result.<br/>ResultCode which shows the state and result from the job you have just run

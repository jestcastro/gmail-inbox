[![npm version](https://badge.fury.io/js/gmail-inbox.svg)](https://badge.fury.io/js/gmail-inbox)
![https://img.shields.io/npm/dm/gmail-inbox](https://img.shields.io/npm/dm/gmail-inbox)

# Gmail-inbox

Gmail-inbox is a simplified gmail API to receive emails in coding. It helps with end-to-end testing your signup process, test email functionality and automate processes that require receiving emails.

### Installation

Install the dependencies 

```sh
$ npm install -S gmail-inbox
```

# Example

Complete examples can be found in the `examples/` folder

##### Receive email example
 &nbsp;
```javascript
import { Inbox } from 'gmail-inbox';

async function exeCuteMe(){
  let inbox = new Inbox('credentials.json');
  await inbox.authenticateAccount(); // logs user in
  
  let messages = await inbox.getInboxMessages();

  console.log("my inbox messages", JSON.stringify(messages,null,4));
  
  // Note: give  https://github.com/ismail-codinglab/gmail-inbox a star if it saved you time!
}

exeCuteMe();
```

# Getting started

### Get gmail API credentials

To work with the gmail API you need to get the credentials from the google cloud console.

**Step 1**
Follow the google instructions to [Create a client ID and client secret](https://developers.google.com/adwords/api/docs/guides/authentication#create_a_client_id_and_client_secret).

**Step 2**
Go to https://console.cloud.google.com/apis/credentials and download the OAuth2 credentials file, as shown in the image below.

![Google cloud platform](https://i.ibb.co/cF00Qxh/image.png)

Your credentials file (commmonly named client_secret_\*.json), should look similar to image below

![credentials file example](https://i.ibb.co/1stgn28/credentials.png)

Note: make sure you selected 'other' as project and that the `redirect_uris` contains something like `"urn:ietf:wg:oauth:2.0:oob"`

**Step 3** Copy the example code in #example and execute the script

**Step 4**
The application will prompt to visit the authorization url. Navigate to the url, select your email and copy the code as shown in the image below. 
![Copy image url](https://i.ibb.co/nrSf7rK/image.png)

Note: The authorization token will only be valid for 6 months, after 6 months a renewal is required.

**Step 5**
Done! You're good to go, you should be able to see your inbox messages, enjoy coding! :)

# API

### Available methods

```Typescript
interface InboxMethods {
  authenticateAccount(): Promise<void>;
  findMessages(searchQuery: SearchQuery);
  getAllLabels(): Promise<Label[]>;
  getInboxMessages(): Promise<Message[]>;
  waitTillMessage(
    searchQuery: SearchQuery,
    shouldLogEvents: boolean,
    timeTillNextCallInSeconds: number,
    maxWaitTimeInSeconds: number
  ): Promise<Message[]>;
}
```

### `Inbox.findMessages(searchQuery: SearchQuery)` / `Inbox.waitTillMessage(searchQuery: SearchQuery, ...)`
Both `findMessages` and `waitTillMessage` support the same searchquery

Since the code is typed in TypeScript I will just include the self-documented interface :)
```typescript
type MessageFilterIsType = 'read' | 'unread' | 'snoozed' | 'starred' | 'important';
interface SearchQuery {
  /**
   * Search for one or multiple potential subjects
   */
  subject?: string | string[];
  message?: string;
  mustContainText?: string | string[];
  from?: string | string[];
  to?: string | string[];
  cc?: string;
  bcc?: string;
  /**
   * In which label the message should be
   */
  labels?: string[];
  has?: 'attachment' | 'drive' | 'document' | 'spreadsheet' | 'youtube' | 'presentation';
  /**
   * Some possible extensions to search with, if not use "filename" property with your extension. e.g. filename: "png"
   * Note: The filenames containing the extension will also be returned. E.g. 'filenameExtension:"pdf" will also return 'not-a-pdf.jpg' 
   */
  filenameExtension?: 'pdf' | 'ppt' | 'doc' | 'docx' | 'zip' | 'rar';
  /**
   * you can search like filename: "pdf" or filename:"salary.pdf"
   */
  filename?: string;
  /**
   * What status the message is in
   */
  is?: MessageFilterIsType | MessageFilterIsType[];

  olderThan?: {
    /**
     * Must be higher than 0
     */
    amount: number;
    period: "day" | "month" | "year"
  },
  newerThan?: {
    /**
     * Must be higher than 0
     */
    amount: number;
    period: "day" | "month" | "year"
  }
  category: "primary" | "social" | "promotions" | "updates" | "forums" | "reservations" | "purchases",
}

```

# Development

Want to contribute? Great!

Help us by creating a pull request

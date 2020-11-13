# samlify &middot;

[![Build Status](https://travis-ci.org/tngan/samlify.svg?branch=master)](https://travis-ci.org/tngan/samlify)
[![npm version](https://img.shields.io/npm/v/samlify.svg?style=flat)](https://www.npmjs.com/package/samlify)
[![Coverage Status](https://img.shields.io/coveralls/tngan/samlify/master.svg)](https://coveralls.io/github/tngan/samlify?branch=master)
[![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/samlify)

Highly configuarable Node.js SAML 2.0 library for Single Sign On

## Welcome PRs

Welcome all PRs for maintaining this project, or provide a link to the repositories especially for use cases alongside with different frameworks.

### Sponsor

| <img width="50" src="https://user-images.githubusercontent.com/83319/31722733-de95bbde-b3ea-11e7-96bf-4f4e8f915588.png"> | <div style="text-align: left;">If you want to quickly implement SAML SSO, feel free to check out Auth0's NodeJS SDK and free plan at [auth0.com/developers](https://auth0.com/developers?utm_source=GHsponsor&utm_medium=GHsponsor&utm_campaign=samlify&utm_content=auth).</div> |
| :----------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |


### Installation

To install the stable version

Starting from v2.6, multiple schema validators are now supported. You can simply set the validator via the following global method. We have four validator modules right now, and you can write your own. The `setSchemaValidator` is required since v2.6, it will throw error if you don't set at the beginning.

```js
import * as samlify from 'samlify';
import * as validator from '@authenio/samlify-xsd-schema-validator';
// import * as validator from '@authenio/samlify-validate-with-xmllint';
// import * as validator from '@authenio/samlify-node-xmllint';
// import * as validator from '@authenio/samlify-libxml-xsd'; // only support for version of nodejs <= 8

// const validator = require('@authenio/samlify-xsd-schema-validator');
// const validator = require('@authenio/samlify-validate-with-xmllint');
// const validator = require('@authenio/samlify-node-xmllint');
// const validator = require('@authenio/samlify-libxml-xsd');

samlify.setSchemaValidator(validator);
```

Now you can create your own schema validator and even suppress it but you have to take the risk for accepting malicious response.

```typescript
samlify.setSchemaValidator({
  validate: (response: string) => {
    /* implment your own or always returns a resolved promise to skip */
    return Promise.resolve('skipped');
  }
});
```

For those using Windows, `windows-build-tools` should be installed globally before installing samlify if you are using `libxml` validator.

```console
yarn global add windows-build-tools
```

### Development

This project is now developed using TypeScript, also support Yarn which is a new package manager.

```console
yarn global add typescript
yarn
```

### Get Started

```javascript
const saml = require('samlify');
```

See full documentation [here](https://samlify.js.org/)

### Example

[react-samlify](https://github.com/passify/react-samlify) SP example powered by React, TypeScript and Webpack

### Talks

[An introduction to Single Sign On](http://www.slideshare.net/TonyNgan/an-introduction-of-single-sign-on)

### License

[MIT](LICENSE)

### Copyright

Copyright (C) 2016-present Tony Ngan, released under the MIT License.


### Added in this version
When initiating iDp you can provide some additional template parameters that allows library to be used with Tableu

```
loginResponseTemplate: {
       context: '',  // use default
       attributes: [ // attributes for template in binding-post, used as AttributeStatement. Uses attributeStatementBuilder from libsaml
         Object.assign({}, attributeFormat, {name: 'username', valueTag: 'UserEmail'}),
         Object.assign({}, attributeFormat, {name: 'fullName', valueTag: 'UserFirstName} {UserLastName'}),
         Object.assign({}, attributeFormat, {name: 'firstName', valueTag: 'UserFirstname'}),
         Object.assign({}, attributeFormat, {name: 'lastName', valueTag: 'UserLastname'}),
         Object.assign({}, attributeFormat, {name: 'email', valueTag: 'UserEmail'}),
         Object.assign({}, attributeFormat, {name: 'mobilePhone', valueTag: 'UserMobilePhone'}),
       ],
       AuthnStatement: { // this is for setting AuthnStatement for binding-post
         content: '<saml:AuthnStatement AuthnInstant="{now}" SessionNotOnOrAfter="{tenHoursLater}" SessionIndex="_asdgerw3w3g43"><saml:AuthnContext><saml:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</saml:AuthnContextClassRef></saml:AuthnContext></saml:AuthnStatement>',
         tags: { // tags can be any object with get. that need to return object with keys, they are used as tags only for AuthnStatement template! 
           get() {
             const n = new Date();
             const tn = new Date(n.getTime());
             const nn = tn.setHours(n.getHours() + 10);
             return {
               now: n.toISOString(),
               tenHoursLater: tn.toISOString(),
             };
           }
         }
       }
     }



------
const attributeFormat = {
  nameFormat: 'urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified',
  valueXsiType: 'xs:string',
  valueXmlnsXs: 'http://www.w3.org/2001/XMLSchema',
  valueXmlnsXsi: 'http://www.w3.org/2001/XMLSchema-instance',
};
```

### Change Authors
 * Andrei Popescu - made changes in build .js files for this work with tableu requirements
 * Artur (Seti) Łabudziński - implemented those changes into .ts files


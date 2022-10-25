---
  type: posts
  title: Changing from cipher to cipheriv
  date: 2018-04-20
---
  
If you have upgraded your version of node.js recently to a version >= 8.9.0 and you are encrypting some data with the standard [crypto](https://nodejs.org/api/crypto.html) library you might have noticed the warning ⚠️:

```
Warning: Use Cipheriv for counter mode of aes-256-ctr
```

### What changed?

Nothing changed in the functionality of the cipher function, Node recently went through a few security enhancements recently and this message is just to inform you that you may not have as tight encryption on your data as you first thought.  when using the [createCipher](https://nodejs.org/api/crypto.html#crypto_class_cipher) function there is a bit of code that checks if you are using aesxxx-ctr or aesxxx-gcm to encrypt data and displays a warning  [here](https://sourcegraph.com/github.com/nodejs/node/-/commit/f3cd53751ba3f917a0996a8f38c991242a8fbc76)

### What do I **need** to do?

Technically nothing. ☕️  But... if you are using ctr encryption without a random IV then the values you are encrypting are with really weak encryption and if you are trying to use encryption I guess you don't want weak encryption. 

### What **should** I do?

Depending on your use case, tighten up your encryption 👮‍ by using [createCipheriv](https://nodejs.org/api/crypto.html#crypto_crypto_createcipheriv_algorithm_key_iv_options) instead of createCipher as the warning message suggests, this involves as the function name suggests using an IV.  You can also change the type of encryption to another [block cipher mode of operation](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation) but I would definately read up on it before you decide.  The easiest, safest option would be to use a random IV.

### What is an IV?

An `IV` is an ancryonmn for a [Initialisation Vector](https://en.wikipedia.org/wiki/Initialization_vector) which is a unique value used during encryption **for each value**.  If a key is used without an IV then it's possible (but quite difficult) to figure out a common way that each value is encrypted as all data would have been encrypted with exact same way.  An IV provides a way to randomise each encryption, basically a [salt](https://en.wikipedia.org/wiki/Salt_(cryptography)) that is stored with the data.

### The downsides

One major downside is the encrypted string can't be searched.  If encrypted with a key only the search term can be encrypted, with a unique iv the value would need to be retrieved before it can be encrypted and there is no way to find that value. 

Processing power / time.  This method of encryption is asynchronous but with an IV the time for encrytion takes longer.

### How do I do it?

It's not too difficult to change your code to add an IV, and will add a 

The following variables are used in the example below, you might have a different input or output encoding and that would change the format of the key.  The input encoding of `utf8` means that I can store the key as an environment variable to avoid checking it in to source control.

```javascript
const algorithm = 'aes-256-ctr';
const key = process.env.KEY || 'b2df428b9929d3ace7c598bbf4e496b2';
const inputEncoding = 'utf8';
const outputEncoding = 'hex';
```

#### Encryption with key only.

The following code will cause the warning above to appear using the `createCipher` function

##### Key only Encryption

```javascript
/**
 * Encrypt function using only the key.
 * @param {string} value to encrypt
 */
module.exports.encrypt = value => {
    const cipher = createCipher(algorithm, key);
    let crypted = cipher.update(value, inputEncoding, outputEncoding);
    crypted += cipher.final(outputEncoding);
    return crypted;
}
```

##### Key only Decryption

```javascript
/**
 * Decrypt function using only the key
 * @param {string} value to decrypt
 */
module.exports.decrypt = value => {
    const decipher = createDecipher(algorithm, key);
    let dec = decipher.update(value, outputEncoding, inputEncoding);
    dec += decipher.final(inputEncoding);
    return dec;
}
```

#### Encryption with key and IV

A **new** random IV is created **on each encryption** and stored with the encrypted valud so it can be decrypted later.  The key you use for the the algorithm `aes-256-ctr` **must be 32 characters** long and the **IV must be a buffer of length 16**.  Note: Other alorithms have different ways of encrypting!

##### Key and IV Encryption

The IV will be stored with the value `<IV value>:<encrypted value>`

```javascript
/**
 * Encrypt using an initialisation vector
 * @param {string} value to encrypt
 */
module.exports.encryptIv = value => {
    const iv = new Buffer(randomBytes(16));
    const cipher = createCipheriv(algorithm, key, iv);
    let crypted = cipher.update(value, inputEncoding, outputEncoding);
    crypted += cipher.final(outputEncoding);
    return `${iv.toString('hex')}:${crypted.toString()}`;
}
```

##### Key and IV decryption

To decrypt, split up the IV stored to the left of the `:` and decrypt the value to the right of the `:` with the IV

```javascript
/**
 * Decrypt using an initialisation vector
 * @param {string} value value to decrypt
 */
module.exports.decryptIv  = value => {
    const textParts = value.split(':');

    //extract the IV from the first half of the value
    const IV = new Buffer(textParts.shift(), outputEncoding);

    //extract the encrypted text without the IV
    const encryptedText = new Buffer(textParts.join(':'), outputEncoding);

    //decipher the string
    const decipher = createDecipheriv(algorithm,key, IV);
    let decrypted = decipher.update(encryptedText,  outputEncoding, inputEncoding);
    decrypted += decipher.final(inputEncoding);
    return decrypted.toString();
}
```

You can find the code with tests in a repo here: https://github.com/peterjgrainger/crypto-example 

I hope you found this useful 😀

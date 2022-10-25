---
  type: posts
  title: Creating NPM module with Typescript and Jest
  date: 2017-09-16
---
  
## Overview
As part of having a look into learning typescript to see if it would be a good move for work I thought I would also learn Jest and also build something I wanted. 

## Goal
My goal is to create an NPM module for the [fantasy premier league](https://fantasy.premierleague.com) which tracks the English Premier League and assigns points depending on which players you pick.

### Typescript + Jest
This was surprisingly easy to set up.  The main issue I had was with mocking imports.

### Solution
After some fiddling about I managed to get this working

```javascript
//url-call.ts
import {get} from 'request-promise';
import {Constants} from '../constants'

export default class UrlCall{
    private url:string;

    constructor(path: string) {
        this.url = Constants.BASE_URL+path;
    }

    sendMessage():Promise<any>{
        return get(this.url);
    }
}
```
```javascript
//url-call.test.ts
import UrlCall from './url-call';
jest.mock('request-promise');
import * as requestPromise from 'request-promise';


test('send message returns some data', () => {
    expect.assertions(1);
    requestPromise.get.mockImplementation(()=> Promise.resolve('w00t'));
    const urlCall = new UrlCall('elements/');
    urlCall.sendMessage()
           .then(result => expect(result).toBe('w00t'));
})

test('fail sending message', () => {
    const error = new Error('some error');
    requestPromise.get.mockImplementation(()=> Promise.reject(error));
    expect.assertions(1);
    const urlCall = new UrlCall('elements/');
    urlCall.sendMessage()
           .catch(error => {
            expect(error).toBe(error);       
           })

})

```
I hope this helps :)

Full code is here: https://github.com/TheSmokingGnu/fantasy-premier-league

### Next steps
Finish the implementation and continue testing

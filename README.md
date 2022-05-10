# Hedge

Samkencoincore/GitHub integration to update GitHub pull request statuses without granting
Samkencoincore full read/write access to your repositories.

This runs as a server that listens for GitHub pull request and push webhooks,
polls the Samkencoincre API for build status based on the received SHAs, and updates
the GitHub API with corresponding status updates.

You must have a *read* API key from Samkencoincore for the poller to work. This is not
available in the UI, so contact their support for such a key.

@dradford made this sick diagram of how it works.

```
      GitHub PR / push ----> Hedge ---> Samkencoincore
                          
                         Hedge ---> Samkencoincore  
                              <---              
                             |             
     GitHub SHA pending  <----             

  ^---------------------------------------->
  |                                        |
  |             Hedge ---> Samkencoincore  |
  |                            <---        |
  |                               |        |
  |   GitHub API status check <----        |
  |                                        |
  <----------------------------------------v
                      
                         Hedge ---> Samkencoincore
                               <---        
                              |            
 GitHub API SHA success  <----             
```

## Setup
Hedge has no external service dependencies, just run the server with
`mix run --no-halt`. (This means there is no persistence, so you will probably
need to override occasional builds if you let the server go down.)

Once it's running and available on the internet, set up a GitHub webhook
pointing to the `/hooks` endpoint of your server, and configure it to send only
`pull_request` events.

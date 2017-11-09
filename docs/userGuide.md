# oysttyer User Guide

         .#*^#=.                
         %'.,`.#`               
       ;',. ./#`                                                   
      ({.`,` #/                 
      `& ,` %,~=*'"*=~=-.,      
       \`=_/'.``  -  `'.  *\.                                      
        (%.  -  -      ˋ-. `&   
        `&`  ~     @      . #                                       
         `\`. `    .....ˊ  %'   
           `^~._.,,,.-+=~*'                                            
                ````'''         
                                
      _      _             _     
     /.\ | ||_  |_ |_ | | /_\|'`                                 
     \_/ \_| _|'|_'|_ \_| \_ |                                   
           |            |       
           

* [Getting started](#start)
    * [Getting the code](#getcode)
    * [Generating your API key](#apikey)
    * [Switching from TTYtter](#switching)
* [Basic usage](#usage)
    * [Commands](#commands)
* [Configuration](#configuration)
* [Feedback and contributing](#contributing)

<h2 id="start">Getting started</h2>

Welcome to [oysttyer](http://oysttyer.github.io/), a console-based Twitter client written in Perl. This guide describes the basic use of oysttyer. If anything is unclear, incorrect, or missing, please [let us know](#contributing).

<h3 id="getcode">Getting the code</h3>

oysttyer is a single Perl script. You will, of course, need a Perl interpreter and also cURL (included by default on most Linux and Unix distributions). You may also want [Term::Readline::TTYtter][TRT] in order to take advantage of some of the more colorful things oysttyer can do.

  [TRT]: http://search.cpan.org/~ckaiser/Term-ReadLine-TTYtter-1.4/

There are two options for getting oysttyer: a "stable" release version and the latest development version. In practice, there is not much difference between the two since new releases happen quickly after bugs are fixed or features are added and development changes are (we hope!) not going to break anything.

To get the stable release, go to the [GitHub repository][repo] and click "Releases" download the most recent release and extract it.

To get the latest development version oysttyer, you can clone the [GitHub repository][repo] or simply download the raw oysttyer.pl file.

  [repo]: https://github.com/oysttyer/oysttyer
  [raw_pl]: https://raw.githubusercontent.com/oysttyer/oysttyer/master/oysttyer.pl
  
<h3 id="apikey">Generating your API key</h3>

Before you can get started, you should generate an API key. The key built in to oysttyer is frequently blocked by Twitter, so using your own key is adviseable.

1. Log in to the (Twitter Application Management page)[https://apps.twitter.com/] with your Twitter user name and password
2. Click *Create New App*
3. Fill out the new app form:
    * Name -- The globally unique name for your application. We suggest something like "oysttyer (YourCoolTwitterHandle)"
    * Description -- Please use "An interactive console text-based command-line Twitter client written in Perl"
    * Website -- Please use "http://oysttyer.github.io/"
    * Callback URL -- (leave blank)
4. Check the box to agree to the developer agreement and click *Create Your Twitter Application*
5. Update your application to be able to send and receive direct messages
    * Click the *Permissions* tab
    * Select *Read, Write and Access direct messages*
    * Click *Update application*
6. Get your keys
    * Click the *Key and Access Tokens* tab
    * Note the *Consumer Key (API Key)* and *Consumer Secret (API Secret)*.

Now that you have your keys, you can authorize oysttyer. Your key and secret can be passed on the command line or in your [configuration file](#configuration). An example of using it on the command line is:

    perl ./oysttyer.pl -oauthkey=nZdXCxVtQTsFJaybAr9OjApd0 -oauthsecret=ujN4ilao663PyDDgObkk0N6EIbAxxxG7MGoK2Yq7nzLLfOxZjE
   
Similarly, the configuration file settings are `oauthkey` and `oauthsecret` for the Consumer Key and Consumer Secret respectively.

A third option is to generate your token and secret from the Application Management website and write that directly to the .oysttyerkey file:

    ck=X&cs=X&at=YOUR_ACCESS_TOKEN&ats=YOUR_ACCESS_TOKEN_SECRET
   
<h3 id="switching">Switching from TTYTter</h3>

If you previously used TTYtter, you should find oysttyer very familiar. A few steps are necessary to make the switch:

1. You have to re-authorise (you can't use your .ttytterkey) as we have a new API key
2. Move/rename your .ttytterc file to .oysttyerrc
3. If you use the ttytteristas pref it is now called oysttyeristas
4. Read the Changelog to see what's new since TTYtter 2.1

<h2 id="usage">Basic usage</h2>

To start using oysttyer, run `perl ./oysttyer.pl` in the directory where you've downloaded osytter.pl. Soon tweets start spewing forth. Each tweet is displayed with an menu code (internal to oysttyer) and the username. For example:

    h8> <FunnelFiasco> I just really like writing documentation.

Menu codes are typically a letter followed by a single-digit number, except direct messages which prepend a d and search results (including threads, `/again`, etc) which prepend a z.

The username may also be prepended with special characters that give you more information about a tweet.

<table id="decorators">
<tr><th>Decorator</th><th>Meaning</th></tr>
<tr><td>@</td><td>Tweet is in reply to another tweet</td></tr>
<tr><td>"</td><td>Tweet quotes the tweet below</td></tr>
<tr><td>↑</td><td>Tweet was quoted by the tweet above</td</tr>
<tr><td>%</td><td>Tweet was retweeted into your timeline</td></tr>
<tr><td>+</td><td>Tweet has location information</td></tr>
</table>

Ready to tweet? Just type your tweet and hit enter. But simply broadcasting is no fun (even if it's your personal #brand). Interaction is what makes twitter enjoyable. The [commands](#commands) section below describes how you can interact. 

<h3 id="commands">Commands</h3>

oysttyer provides many commands, all of which begin with `/`. For the sake of your followers, anything that starts with `/` is treated as a command and oysttyer will let you know if it doesn't recognize it. If you want to start a tweet with `/`, double it (`//`). The tables below describe most of the oysttyer commands, in hopefully-helpful sections:

* [oysttyer commands](#commands-oysttyer)
* [tweet commands](#commands-tweet)
* [user commands](#commands-user)
* [list commands](#commands-list)

Commands that return multiple responses default to 20, but that can be adjusted by specifying +*N* after the command name. For example:

    /dmagain +5
    
will only give you your last 5 direct messages, instead of the last 20.

<h4 id="commands-oysttyer">oysttyer commands<h4>
<table>
<tr><th>Command</th><th>Description</th></tr>
<tr><td>/! *shell command*</td><td>Run *shell command* in a subprocess</td></tr>
<tr><td>/add <em>key</em> <em>value</em></td><td>Add <em>value</em> to <em>key</em> (see the [runtime configuration section](#runtime) for more information)</td></tr>
<tr><td>/cls</td><td>Clear the screen</td>
<tr><td>/del <em>key</em> <em>value</em></td><td>Remove <em>value</em> from <em>key</em> (see the [runtime configuration section](#runtime) for more information)</td></tr>
<tr><td>/help</td><td>Display help</td></tr>
<tr><td>/history</td><td>Display your command history</td></tr>
<tr><td>/pop <em>key</em></td><td>Pop the first value off of <em>key</em> (see the [runtime configuration section](#runtime) for more information)</td></tr>
<tr><td>/print <em>key</em></td><td>Display all settings (or the value of <em>key</em>, if specified)</td></tr>
<tr><td>/push <em>key</em> <em>value</em></td><td>Push <em>value</em> onto the stack for <em>key</em> (see the [runtime configuration section](#runtime) for more information)</td></tr>
<tr><td>/ruler</td><td>Print a "ruler" of 140 characters</td></tr>
<tr><td>/set <em>key</em> <em>value</em></td><td>Set <em>key</em> to <em>value</em> (see the [runtime configuration section](#runtime) for more information)</td></tr>
<tr><td>/unset <em>key</em></td><td>Unsets <em>key</em>(see the [runtime configuration section](#runtime) for more information)</td></tr>
<tr><td>/vcheck</td><td>Check to see if you're running the latest version</td></tr>
<tr><td>/quit</td><td>Exit oysttyer</td></tr>
</table>

<h4 id="commands-tweet">Tweet commands</h4>
<table>
<tr><th>Command</th><th>Description</th></tr>
<tr><td>/re <em>menu code</em></td><td>Reply to tweet <em>menu code</em></td></tr>
<tr><td>/ra <em>menu code</em></td><td>Reply to all users in tweet <em>menu code</em></td></tr>
<tr><td>/rt <em>menu code</em> <em>message</em></td><td>Retweet tweet <em>menu code</em>. If a message is specified, quote tweet <em>menu code</em></td><tr>
<tr><td>/like <em>menu code</em></td><td>Like tweet <em>menu code</em></td></tr>
<tr><td>/unlike <em>menu code</em></td><td>Remove like for tweet <em>menu code</em></td></tr>
<tr><td>/lrt <em>menu code</em></td><td>Like and retweet tweet <em>menu code</em></td></tr>
<tr><td>/thread <em>menu code</em></td><td>Displays the thread for tweet <em>menu code</em></td></tr>
<tr><td>/again <em>username</em></td><td>Displays the recent tweets for <em>username</em> (if none is specified, shows recent tweets in your timeline again)</td></tr>
<tr><td>/deletelast</td><td>Deletes your last tweet</td></tr>
<tr><td>/delete <em>menu code</em></td><td>Deletes tweet <em>menu code</em></td></tr>
<tr><td>/dump <em>menu code</em></td><td>Display all data for tweet <em>menu code</em></td></tr>
<tr><td>/dm <em>username</em> <em>message</em></td><td>Send a direct message to <em>username</em></td></tr>
<tr><td>/dmsent</td><td>Display direct messages you have sent</td></tr>
<tr><td>/dmagain</td><td>Display direct messages sent you you</td></tr>
<tr><td>/dmrefresh</td><td>Check for new direct messages</td></tr>
<tr><td>/entities <em>menu code</em></td><td>Display the expanded t.co links in tweet <em>menu code</em></td></tr>
<tr><td>/search <em>keyword</em></td><td>Search public tweets for <em>keyword</em></td></tr>
<tr><td>/refresh</td><td>Check for new tweets</td></tr>
<tr><td>/rtsof <em>menu code</em></td><td>List accounts that retweeted tweet <em>menu code</em></td></tr>
<tr><td>/rtsofme</td><td>List your tweets that others have retweeted</td></tr>
<tr><td>/track <em>keywords</em></td><td>Search for <em>keywords</em> in addition to your normal timeline</td></tr>
<tr><td>/tron <em>keywords</em></td><td>Add additional keywords to <code>/track</code></td></tr>
<tr><td>/troff <em>keywords</em></td><td>Stop <code>/track</code>ing <em>keywords</em></td></tr>
<tr><td>/trends</td><td>List the current trending topics</td></tr>
<tr><td>/url <em>menu code</em></td><td>Display the URLs in tweet <em>menu code</em> and open in the application defined by the <code>urlopen</code> option</td></tr>
</table>

<h4 id="commands-user">User commands</h4>
<table>
<tr><th>Command</th><th>Description</th></tr>
<tr><td>/follow <em>username</em></tdn><td>Start following <em>username</em></td></tr>
<tr><td>/unfollow <em>username</em></td><td>Stop following <em>username</em></td></tr>
<tr><td>/leave <em>username</em></td><td>Alias for <code>/unfollow</code></td></tr>
<tr><td>/block <em>username</em></td><td>Block <em>username</em></td></tr>
<tr><td>/unblock <em>username</em></td><td>Un-block <em>username</em></td></tr>
<tr><td>/mute <em>username</em></td><td>Mute <em>username</em></td></tr>
<tr><td>/unmute <em>username</em></td><td>Un-mute <em>username</em></td></tr>
<tr><td>/whois <em>username</em></td><td>Display information for <em>username</em></td></tr>
<tr><td>/doesfollow <em>username</em> <em>seconduser</em></td><td>Check if a user follows another (if a second account is not specified, it checks if the user follows you)</td></tr>
<tr><td>/friends <em>username</em></td><td>Get a list of the user's friends (if an account is not specified, it lists your friends)</td></tr>
<tr><td>/followers <em>username</em></td><td>Get a list of the user's followers (if an account is not specified, it lists your followers)</td></tr>
</table>

<h4 id="commands-list">List commands</h4>

This section is not yet written

<h2 id="configuration">Configuration</h2>

This section is not yet written

<h2 id="contributing">Contributing</h2>

This section is not yet written
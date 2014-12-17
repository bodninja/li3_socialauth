li3_socialauth
==============

> I'm sorry, this li3 package isn't still maintained. If someone wants to take the lead, just ping me.

Lithium universal oAuth plugin: uses the excellent oAuth library by Lusitanian (https://github.com/Lusitanian/PHPoAuthLib) to provide Li3 auth adapters. 

Installation
------------

The easiest way to install li3_socialauth is to use composer, adding this lines in your composer.json file:

    {
        "require": {
            "scharrier/li3_socialauth": "dev-master"
        }
    }

Then update your project:

    composer update

And load the library:

    // config/bootstrap/libraries.php
    Libraries::add('li3_socialauth') ;
    
    // Add the composer autoloader if not already done
    require_once(LITHIUM_LIBRARY_PATH . '/autoload.php') ;


Using li3_socialauth
--------------------

Each social auth type has its own Lithium auth adapter. Just define your new auth like this:

    Auth::config(array(
      'twitter' => array(
      	'adapter' => 'Twitter',
          'key' => 'YOUR_KEY',
          'secret' => 'YOUR_SECRET'
      ),
      'facebook' => array(
        'adapter' => 'Facebook',
          'key' => 'YOUR_KEY',
          'secret' => 'YOUR_SECRET',
          'scope' => array('email','read_friendlists','user_online_presence')   // Extend the auth scope
      )
    ));

Then you can use this auth like any other auth mecanism in Lithium. Here is an example of an oauth login method:

    public function login() {
        // The user choose an external auth : adapter name given as the first param
        if (!empty($this->request->params['args'][0])) {
            // Can we login in ?
    		if (Auth::check($this->request->params['args'][0], $this->request)) {
    			$this->redirect('/') ;
    		}
    	}
    }

And you are done.

Universal Auth class
--------------------

Using this method of authentication, you will quickly have a lot of adapters to check in order to get the authed user. So I have created an universal Auth class witch checks each adapter when calling check() or clear() methods.

Here is an example of usage:

    // Using li3_socialauth instead of Lithium auth
    use li3_socialauth\extensions\security\Auth;
    
    if (Auth::check()) {
        echo 'Logout ?' ;
    } else {
        echo 'You should login first' ;
    }
    
Supported services
------------------

Actually these services are supported. More to come soon:
- Twitter
- Facebook
- Google+
- Microsoft Live
- Github
- Foursquare

Help and support
----------------

Fork it, play with it, commit and pull ! Email at scharrier@gmail.com if some help needed.

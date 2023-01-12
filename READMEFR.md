# Nintendo Switch Online Rich Presence

*Affiche ton statut de jeu sur Discord !*

Les trucs à lire:
  - [The quickstart guide](#quick)
  - [In-depth guide](#depth)

### Credits

Ce projet utilise l'API Nintendo Switch Online Mobile App.  
Des remerciments à :
- le(s) developpeur(s) de [NintendoSwitchRESTAPI](https://github.com/ZekeSnider/NintendoSwitchRESTAPI) (très cool)
- [frozenpandaman](https://github.com/frozenpandaman) et son API [s2s][s2s] (c'est le truc qui fait que ça marche)
- [JoneWang](https://github.com/JoneWang) et sont API [imink][imink]. C'est des trucs important pour l'authentification.
- [blackgear](https://github.com/blackgear)'s [NSOnline_API](https://github.com/blackgear/NSOnline_API) (C'est lui qui fait tout les trucs à base de `session_token` pour l'authentication)
- [qwerty](https://github.com/qwertyquerty) pour [pypresence](https://github.com/qwertyquerty/pypresence)
- [samuelthomas2774](https://github.com/samuelthomas2774) pour l'aide incroyable qui à fournit pour ce  projet, checkez  [nxapi ici](https://github.com/samuelthomas2774/nxapi)!
- [anthonybaldwin](https://github.com/anthonybaldwin) pour son aide aussi d'incroyable qualité finalement.
- [LyKøs](https://github.com/LyKosDeluxe) Pour les traductions en Français de tout l'bazar qu'est ce projet finalement, à bon entendeur 🙏

<h1 id = 'quick'>Guide de démarrage</h1>

Téléchargez  [la dernière version](https://github.com/MCMi460/NSO-RPC/releases) et exécutez-là!  
Une fois l'appli ouverte, elle vous demandera de vous connectez à votre compte Nintendo depuis le navigateur. Y'a pas de mal, on va pas piqué tes codes.. le mieux c'est que tu vérifie [le code source][api] par toi-même.

1. Ouvrir Discord & NSO-RPC

  - t'auras besoin d'un compte secondaire pour "Sélectionner" le profil que tu voudras afficher en statut sur Discord. Du au changement récent de l'API Nintendo ([#13](https://github.com/MCMi460/NSO-RPC/issues/13)), C'est impossible d'extruder ton status du coup on va tout contourner, tu peux créer un nouveau compte Nintendo auquel tu devras ajouter ton compte principal en ami, ou alors si t'as des vrais potos, utiliser leurs comptes ou celui de ta meuf pour te connecter avec dans NSO-RPC.

2. Connecte-toi avec le compte secondaire

3. Clic droit sur 'Selectionner' et fait 'Copier l'adresse du lien'

![link](/resources/link.png)

4. Copie-le dans NSO-RPC et clique sur 'Se connecter'

5. Après t'être connecter, va dans "Liste d'Amis" et sélectionne ton compte Perso/Principal

6. Clique en bas sur "Sélectionner ce profil". NSO-RPC devrait se fermer, relance-le.

7. Essaye. Lance un jeu et regarde si tout fonctionne.

![display](/resources/display.png)

## FAQ

> If none of the below Qs and As help with your problem, feel free to [file an issue](https://github.com/MCMi460/NSO-RPC/issues/new). Alternatively, you can join the [NSO-RPC Discord server](https://discord.gg/pwFASr2NKx) for a better back-and-forth method of communication with me!

***Q: Y'a t-il besoin d'avoir un abonnement Nintendo Switch Online ?***  
**A:** Non.

***Q: Mon PC me dis que c'est une app malveillante, que dois-je faire ?***  
**A:** Sachez que ce n'est pas le cas, si jamais vous avez des doutes, tout le code est public et accessible via ce projet GitHub. Votre PC vous indique que ce fichier n'est pas de confiance car il n'est pas signé par un certificat que Windows connaît. Si vous avez vraiment peur vous pouvez [créer votre propre `exe`](#building).

***Q: Tu n'est pas entrain de voler mes données hein ? Pas vrai ?***  
**A:** ~~Pas moi, Personnellement. Faut demander à [frozenpandaman](https://github.com/frozenpandaman) [(s2s)][s2s] & [@NexusMine (flapg)](https://twitter.com/NexusMine). C'est eux les responsable de l'authentification.~~ Ce projet utilise [imink API][imink] pour ce qui concerne les étapes d'authentifications. [En savoir Plus ici](#understanding)
<ul><li><details>
  <summary><b><i>Et si j'ai pas envie d'utiliser ImInk ?</i></b></summary>

  **A**: C'est possible de bidouiller le code et de retirer les requêtes d'API, mais du coup vous utiliserais des jetons d'auth qui durerons 2H environ. Vous pouvez utiliser ce projet [mitmproxy](https://github.com/mitmproxy/mitmproxy) pour vous aider.

</details></li></ul>

***Q: Est-ce que j'ai besoin d'avoir Discord ouvert sur mon PC?***  
**A:** Oui. Discord doit-être lancé sur l'ordinateur qui possède NSO-RPC, pas besoin de vos identifiants Discord, même pas besoin d'avoir la fenêtre Discord active !

***Q: I can't get the program to run, what's wrong with it?!***  
**A:** Delete the NSO-RPC folder in your Documents folder. If that doesn't work, you should run the [cli.py][cli] program and get the error data, then make an [issue](https://github.com/MCMi460/NSO-RPC/issues) on Github and I'll investigate it.

***Q: I can't link my Nintendo Account. What do I do?***  
**A:** Refer to the question above.

*I am not liable for any sort of rate limiting Nintendo may hammer upon your network*

<h1 id = 'depth'>In-depth guide</h1>

<h2 id = 'building'>Building</h2>

For Windows, run
```bat
cd .\NSO-RPC\scripts
.\build.bat
```
For MacOS, run
```sh
cd ./NSO-RPC/scripts
chmod +x build.sh
./build.sh
```
For Linux (Ubuntu), run
```sh
cd ./NSO-RPC/scripts
chmod +x install.sh
./install.sh
```

*(Make sure you have `python3` and `pip` installed)

<h2 id = 'understanding'>Understanding</h2>

This is going to be a detailed explanation on everything that I do here and exactly what is going on. If you're into that sort of stuff, keep reading. If not, feel free to skim and get a general idea of the procedures.  
I try my best to be detailed and give a proper comprehensive guide, but I'm not perfect. Feel free to make an [issue](https://github.com/MCMi460/NSO-RPC/issues) if you feel anything in particular should be updated!

I'm going to be explaining my [cli.py][cli] as it isn't as complicated as the [GUI (app.py)][app].  
(You can follow along with the guide [here][api] and [here][cli])  

**As of [8abf86c](https://github.com/MCMi460/NSO-RPC/commit/8abf86c6f4dca2d5cde7bf0886de6f1642b6dbef), this guide is outdated in regards to the APIs used.**

<details>
  <summary><h3>1. Getting your <code>session_token</code></h3></summary>

  First things first, we need to get access to your Nintendo account. What we need to get is your `session_token`, which is a unique identifier that confirms to Nintendo servers *you are you*. This is the code that gets your `session_token`.  
  [cli.py][cli]:
  ```python
  path = os.path.expanduser('~/Documents/NSO-RPC/private.txt')
    if not os.path.isfile(path):
        session = Session()
        session_token = session.run(*session.login(session.inputManually))
    else:
        with open(path, 'r') as file:
            session_token = json.loads(file.read())['session_token']
  ```
  First, it checks if you already have a `session_token` saved. If so, then it just uses that.  
  If not, then it will create a `Session()` object and call `Session().login()` (passing `Session().inputManually`) `Session().run()`.  
  That's all fine and dandy, but what does it do behind the `Session().login()` and `Session.run()` functions?  
  Glad you asked.

  - `Session().__init__()`:

    First, it sets some default headers and creates a `requests.Session()` (this is from the common Python library, [requests](https://github.com/psf/requests)).
    ```python
    self.headers = {
      'Accept-Encoding': 'gzip',
      'User-Agent': 'OnlineLounge/%s NASDKAPI Android' % nsoAppVersion,
    }
    self.Session = requests.Session()
    ```

  - `Session().login()`:

    Now, we create some variables (as dictated from [s2s](https://github.com/frozenpandaman/splatnet2statink/blob/master/iksm.py)) for authorization. Basically just a bunch of random characters, but your guess is honestly as good as mine when it comes down to it, as I'm not an expert on oauth authentication.
    ```python
    state = base64.urlsafe_b64encode(os.urandom(36))
    verify = base64.urlsafe_b64encode(os.urandom(32))
    authHash = hashlib.sha256()
    authHash.update(verify.replace(b'=', b''))
    authCodeChallenge = base64.urlsafe_b64encode(authHash.digest())
    ```
    Here, it sets up authentication form, queries it, gets the URL, and opens it in the user's web browser.
    ```python
    url = 'https://accounts.nintendo.com/connect/1.0.0/authorize'
    params = {
      'client_id': client_id,
      'redirect_uri': 'npf%s://auth' % client_id,
      'response_type': 'session_token_code',
      'scope': 'openid user user.birthday user.mii user.screenName',
      'session_token_code_challenge': authCodeChallenge.replace(b'=', b''),
      'session_token_code_challenge_method': 'S256',
      'state': state,
      'theme': 'login_form'
    }
    response = self.Session.get(url, headers = self.headers, params = params)

    webbrowser.open(response.history[0].url)
    ```
    Finally, it comes to the user's input. We `re.compile()` the proper format of a return token (thank you, [blackgear](https://github.com/blackgear)). Then, using the input method specified in `Session().login()`, we receive the user's URL and `re.findall()` for the proper code.  
    We'll then return the `code` and `verify` variables.
    ```python
    tokenPattern = re.compile(r'(eyJhbGciOiJIUzI1NiJ9\.[a-zA-Z0-9_-]*\.[a-zA-Z0-9_-]*)')
    code = tokenPattern.findall(receiveInput())[0]

    return code, verify
    ```

  - `Session().inputManually()`:

    `Session().inputManually()` is literally just a redirect of the Python `input()` function:
    ```python
    def inputManually(self):
      return input('After logging in, please copy the link from \'Select this account\' and enter it here:\n')
    ```

  - `Session().run()`:

    `Session().run()` returns the `session_token` in a finally usable format:
    ```python
    url = 'https://accounts.nintendo.com/connect/1.0.0/api/session_token'
    headers = self.headers
    headers.update({
      'Accept-Language': 'en-US',
      'Accept':          'application/json',
      'Content-Type':    'application/x-www-form-urlencoded',
      'Content-Length':  '540',
      'Host':            'accounts.nintendo.com',
      'Connection':      'Keep-Alive',
    })
    body = {
      'client_id': client_id,
      'session_token_code': code,
      'session_token_code_verifier': verify.replace(b'=', b''),
    }
    response = self.Session.post(url, data = body, headers = headers)
    return json.loads(response.text)['session_token']
    ```

</details>

<details>
  <summary><h3>2. Connecting to Discord</h3></summary>

  We create a `Discord()` object and pass the newly obtained `session_token` (and `user_lang`) to it. This does not involve sending your `session_token` to Discord.  
  [cli.py][cli]:
  ```python
  client = Discord(session_token, user_lang)
  client.background()
  ```

  - `Discord().__init__()`:

    First, it creates a `pypresence.Presence()` object and passes it my Discord Application ID (this has nothing important other than the name 'Nintendo Switch'; you can replace it with your own ID if you want)  
    Then, it calls `Discord().connect()` to connect to the Discord client.  
    We set the `Discord().running` and `Discord().gui` variables to `False`, then if the parameters `session_token` and `user_lang` are passed, it will call `Discord().createCTX()`.
    ```python
    self.rpc = None
    if rpc:
        if not self.connect():
            sys.exit()
    self.running = False
    self.api = None
    self.gui = False
    if session_token and user_lang:
        self.createCTX(session_token, user_lang)
    ```

  - `Discord().createCTX()`:

    This function just creates an `API()` object and sets it to `Discord().api`. It also sets `Discord().running` to `True`.  
    It requires a `session_token` and a `user_lang` to be passed.
    ```python
    try:
      self.api = API(session_token, user_lang)
    except Exception as e:
      sys.exit(log(e))
    self.running = True
    ```

  - `Discord().connect()`:

    If this errors over 500 times, the application closes.
    ```python
    self.rpc = pypresence.Presence('637692124539650048')
    fails = 0
    while True:
      # Attempt to connect to Discord. Will wait until it connects
      try:
        self.rpc.connect()
        break
      except Exception as e:
        fails += 1
        if fails > 500:
          sys.exit(log('Error, failed after 500 attempts\n\'%s\'' % e))
        continue
    ```
    - `Discord().disconnect()`:

      Closes rich presence connection.
      ```python
      if self.rpc:
          self.rpc.close()
      self.rpc = None
      ```

  - `Discord().setApp()`:

    This is only called by [GUI][app]. All it does is set the usable app function and assign `Discord().gui` to `True`.
    ```python
    def setApp(self, function):
        self.app = function
        self.gui = True
    ```

  - `Discord().update()`:

    This updates the user's Discord Rich Presence. Will error if an `API()` object is not defined at `Discord().api`  
    It basically just calls the API to grab the user's info, then if they are not currently offline, it will update the `Discord().rpc`.  
    If it cannot get the user, it will attempt to login.  
    If they are offline, then it will clear their status.  
    If a `Game().sysDescription` is available, it will display that as the Discord state instead of hours played.  
    If `Discord().gui` is `True`, it will run `Discord().app()`
    ```python
    for i in range(2):
        try:
            self.api.getSelf()
            break
        except Exception as e:
            log(e)
            if i > 0 or time.time() - self.api.login['time'] < 7170:
                raise Exception('Cannot get session token properly')
            self.api.updateLogin()
            continue
    self.nickname = self.api.userInfo['nickname']
    self.user = self.api.user

    presence = self.user.presence
    if presence.game.name: # Please file an issue if this happens to fail
        state = presence.game.sysDescription
        if not state:
            state = 'Played for %s hours or more' % (int(presence.game.totalPlayTime / 60 / 5) * 5)
            if presence.game.totalPlayTime / 60 < 5:
                state = 'Played for a little while'
        self.rpc.update(details = presence.game.name, large_image = presence.game.imageUri, large_text = presence.game.name, state = state)
    else:
        self.rpc.clear()
    # Set GUI
    if self.gui:
        self.app(self.user)
    ```

  - `Discord().background()`:

    This is the background task that runs the entire application. What we do here is that we update the user's status once every 30 seconds. And, uh, that's pretty much it. If `Discord().running` is not `True` then it will set the next update to be 5 seconds after `Discord().running` becomes `True` again (whenever you toggle the Discord option in the taskbar, this is what happens).
    ```python
    second = 30
    while True:
        if self.running:
            if second == 30:
                try:
                    self.update()
                except Exception as e:
                    sys.exit(log(e))
                second = 0
            second += 1
        else:
            second = 25
        time.sleep(1)
    ```

  - `Discord().logout()`:

    Removes the configs in the config folder.
    ```python
    path = os.path.expanduser('~/Documents/NSO-RPC')
    if os.path.isfile(os.path.join(path, 'private.txt')):
        try:os.remove(os.path.join(path, 'private.txt'))
        except:pass
        try:os.remove(os.path.join(path, 'settings.txt'))
        except:pass
        sys.exit()
    ```

</details>

<details>
  <summary><h3>3. Nintendo's API</h3></summary>

  Oh boy.

  Alright, this gets complicated, but I'll try and cover it all quickly.  
  *For code snippets, see [api/\_\_init\_\_.py][api]

  - `API()`:

    Has five functions: `API().__init__()`, `API().makeRequest()`, `API().updateLogin()`, `API().getSelf()`, and `API().getFriends()`.  

    - `API().__init__()`:

      This sets some headers to `API().headers` and assigns `Nintendo().getServiceToken()` to `API().tokenResponse` after passing `session_token` to it.  
      Of all of the important things it retrieves, we only use `API().tokenResponse['access_token']`. We assign that to the 'Authorization' header.
      ```python
      self.headers['Authorization'] = 'Bearer %s' % self.accessToken # Add authorization token
      ```
      We also create a GUID (`uuid.uuid4()`)  
      We set the default URL that isn't really used, then we set `API().userInfo` to `UsersMe().get()`, which used in `API().updateLogin()`.  
      After that, we store the token in plaintext form in your `Documents/NSO-RPC` folder. This will likely not be changed as other methods are not really more secure.

    - `API().makeRequest()`:

      Makes a request to `https://api-lp1.znc.srv.nintendo.net` with a route specified.
      ```python
      def makeRequest(self, route):
        return requests.post(self.url + route, headers = self.headers)
      ```

    - `API().updateLogin()`:

      All this does is create/refresh your `Login()`. It will check a file in your `Documents/NSO-RPC` folder for an already existing temporary token so as to prevent excessive calling of the [s2s API][s2s].  
      See `Login()` for more information.
      ```python
      path = os.path.expanduser('~/Documents/NSO-RPC/tempToken.txt')
      if os.path.isfile(path):
          with open(path, 'rb') as file:
              self.login = pickle.loads(file.read())
              self.headers['Authorization'] = 'Bearer %s' % self.login['login'].account['result'].get('webApiServerCredential').get('accessToken')
              log('Login from file')
      if time.time() - self.login['time'] < 7170:
          return
      login = Login(self.userInfo, self.user_lang, self.accessToken, self.guid)
      login.loginToAccount()
      self.headers['Authorization'] = 'Bearer %s' % login.account['result'].get('webApiServerCredential').get('accessToken') # Add authorization token
      self.login = {
          'login': login,
          'time': time.time(),
      }
      with open(path, 'wb') as file:
          file.write(pickle.dumps(self.login))
      ```

    - `API().getSelf()`:

      This makes a request for user data and assigns it to the `API().user` variable
      ```python
      route = '/v3/User/ShowSelf'

      response = self.makeRequest(route)
      self.user = User(json.loads(response.text)['result'])
      ```

    - `API().getFriends()`:

      This makes a `FriendList()` object and calls `FriendList().populateList()`, then assigns `FriendList().friendList` to `API().friends`
      ```python
      list = FriendList()
      list.populateList(self)
      self.friends = list.friendList
      ```

  - `Nintendo()`:

    This just makes an API call to Nintendo for a token. [Read more here](https://github.com/ZekeSnider/NintendoSwitchRESTAPI/blob/master/NintendoAccountBlueprint.md#service-token-connect100apitoken)

    - `Nintendo().__init__()`:

      Set a bunch of headers and the body of our request. Requires `session_token`.

    - `Nintendo().getServiceToken()`:

      Actually make the request, and return it in `JSON`.

  - `UsersMe()`:

    This gets vital information for the `Login()` class. It's one step before actually logging in.

    - `UsersMe().__init__()`:

      Sets headers and host url. Takes `accessToken` (different from `session_token`).

    - `UsersMe().get()`:

      Very original function name, but it just makes the request. It returns necessary information in `JSON` format, including the user's date of birth, country, and language.

  - `Login()`:

    - `Login().__init__()`:

      Takes `userInfo, userLang, accessToken, guid`.  
      Sets headers, URL, GUID, user's info, `accessToken`, `Flapg()` API, and the user's account.

      Please take extreme caution and note of this piece of code.
      ```python
      self.flapg = Flapg(self.accessToken, self.timestamp, self.guid).get()
      ```

    - `Login().loginToAccount()`:

      Pretty neat. `/v3` is necessary for the Presence information.
      ```python
      route = '/v3/Account/Login'
      body = {
        'parameter': {
          'f': self.flapg['f'],
          'naIdToken': self.flapg['p1'],
          'timestamp': self.flapg['p2'],
          'requestId': self.flapg['p3'],
          'naCountry': self.userInfo['country'],
          'naBirthday': self.userInfo['birthday'],
          'language': self.userInfo['language'],
        },
      }
      response = requests.post(self.url + route, headers = self.headers, json = body)
      self.account = json.loads(response.text)
      return self.account
      ```

  - `Flapg()`:

    [Learn more about this here](https://github.com/frozenpandaman/splatnet2statink/wiki/api-docs#the-flapg-api)  
    This is where it can get risky. We are sending off the user's `accessToken` (a temporary token) to not one, but two third-party APIs. This is what I mentioned in the FAQ about being weary to use this program. It is ran by [@NexusMine on Twitter](https://twitter.com/NexusMine).  
    It is, however, necessary in order to call the `/v3/Account/Login` API, as it retrieves an important factor: The `f` token.  
    Take particular notice of the `s2s()` call.

    - `Flapg().__init__()`:

      Takes `id_token, timestamp, guid`.
      ```python
      self.headers = {
        'x-token': id_token,
        'x-time': str(timestamp),
        'x-guid': guid,
        'x-hash': s2s(id_token, timestamp).getHash(),
        'x-ver': '3',
        'x-iid': 'nso',
      }

      self.url = 'https://flapg.com'
      ```

    - `Flapg().get()`:

      This just connects to the flapg API and returns the result.
      ```python
      def get(self):
        route = '/ika2/api/login?public'

        response = requests.get(self.url + route, headers = self.headers)
        return json.loads(response.text)['result']
      ```

  - `s2s()`:

    [Learn more about this here][s2s]  

    - `s2s().__init__()`:

      Takes `id_token, timestamp`.
      ```python
      log('Login from Flapg/s2s')
      self.headers = {
        'Content-Type': 'application/x-www-form-urlencoded',
        'User-Agent': 'NSO-RPC/%s' % version,
      }
      self.body = {
        'naIdToken': id_token,
        'timestamp': timestamp,
      }
      self.url = 'https://elifessler.com'
      ```

    - `s2s().getHash()`:

      ```python
      route = '/s2s/api/gen2'
      response = requests.post(self.url + route, headers = self.headers, data = self.body)
      return json.loads(response.text)['hash']
      ```

  - `FriendList()`:

    Creates and stores a list of `Friend()` objects

    - `FriendList().__init__()`:

      Defines route and assigns empty list
      ```python
      self.route = '/v3/Friend/List' # Define API route

      self.friendList = [] # List of Friend object(s)
      ```

    - `FriendList().populateList()`:

      Requires the passing of an `API()` object.  
      Calls `API().makeRequest()` with `FriendList().route`, then assigns the results as `Friend()` objects to `FriendList().friendList`
      ```python
      response = API.makeRequest(self.route)
      arr = json.loads(response.text)['result']['friends']
      self.friendList = [ Friend(friend) for friend in arr ]
      ```

  - `User()`:

    This creates an easy-to-use object with the user's data sorted and everything! It's purely for ease-of-use for me.

    - `User().__init__()`:

      Assigns variables from the `JSON` value it accepts as `f`.  
      Calls `Presence()`

    - `User().description()`:

      Unused.  
      Returns a Python string with a quick description of the `User()` object.

  - `Friend()`:

    An object used in tandem with `FriendList()`. Imagine a retexture of the `User()` class, but with the following additions:
    - `Friend().isFriend`
    - `Friend().isFavoriteFriend`
    - `Friend().isServiceUser`
    - `Friend().friendCreatedAt`

  - `Presence()`:

    Creates a presence state.  
    Calls `Game()`

  - `Game()`:

    Sorts game data into a neat little class.

</details>

<details>
  <summary><h3>4. The <code>f</code> token</h3></summary>

  This hurts me. This is the reason why we have to call third-party APIs in order to 'login' to Nintendo. It essentially just verifies that you are connecting from a real Nintendo Switch Online Mobile app (ineffectively, obviously).  
  Since what's required to generate it is potentially incriminating, we have to generate it using third-party APIs (namely [s2s][s2s] and [flapg](https://github.com/frozenpandaman/splatnet2statink/wiki/api-docs#the-flapg-api)).

</details>

[cli]: /client/cli.py
[api]: /client/api/__init__.py
[app]: /client/app.py
[s2s]: https://github.com/frozenpandaman/splatnet2statink/wiki/api-docs
[imink]: https://github.com/JoneWang/imink

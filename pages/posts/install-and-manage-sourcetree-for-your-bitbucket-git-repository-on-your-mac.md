---
  type: posts
  title: Install and Manage Sourcetree for Your Bitbucket Git Repository on Your Mac
  date: 2020-02-03
---
  
Setting up a project is both stressful and fun! One of the first decisions you'll make is where to store your code. Somewhere that's easy to use and easy to integrate with other tools normally tops my list. I use Bitbucket and Sourcetree for many of my projects and urge you to give them a go.

This post will cover installing Sourcetree, connecting Sourcetree to a Bitbucket account, and creating a Git repository with Sourcetree using a Mac. This guide assumes you've already set up a Bitbucket repository. If you haven't already done this, first check out Atlassian's <a href="https://confluence.atlassian.com/bitbucket/create-a-git-repository-759857290.html">comprehensive guide to creating a Git repository</a>.
<h2>A Definition of Git</h2>
Git's <a href="https://git-scm.com/docs/git.html">own manual</a> describes itself as "the stupid content tracker." The README file in the Git <a href="https://github.com/git/git/blob/e83c5163316f89bfbde7d9ab23ca2e25604af290/README">source code</a> describes it in several ways, depending on your mood:
<ul>
 	<li><em>[A] random three-letter combination that is pronounceable, and not actually used by any common UNIX command. The fact that it is a mispronunciation of "get" may or may not be relevant.</em></li>
 	<li><em>Stupid. Contemptible and despicable. Simple. Take your pick from the dictionary of slang.</em></li>
 	<li><em>"Global information tracker": you're in a good mood, and it actually works for you. Angels sing, and a light suddenly fills the room.</em></li>
</ul>
<img class="aligncenter size-large wp-image-17993" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-18-at-14.29.07-1024x400.png" alt="screenshot showing the manual page for Git" width="1024" height="400" />

These descriptions aren't meant to be taken seriously and are just a bit of fun. However, there's some truth to those descriptions—Git <em>can</em> be a little frustrating at times. However, the official description from <a href="https://git-scm.com/">git-scm.com</a> may give you a better idea of what Git is used for: "Git is a version control system for tracking changes in computer files and coordinating work on those files among multiple people."
<h2>An Explanation of Sourcetree and Bitbucket</h2>
<a href="https://www.sourcetreeapp.com/">Sourcetree</a> is a user interface on top of the <a href="https://git-scm.com/book/en/v2/Getting-Started-The-Command-Line">Git command line interface</a>. It's a great way to start learning Git. It helps by doing the following:
<ul>
 	<li>Displaying a visual representation of your Git Repository, both local and remote.</li>
 	<li>Organizing the visibility of commands so the most common Git commands are the easiest to select.</li>
 	<li>Providing guidance on which commands are possible to perform at any stage of the Git workflow by enabling and disabling buttons.</li>
</ul>
<a href="https://bitbucket.org/">Bitbucket</a> is a web-based version control repository hosting service, owned by Atlassian, for source code and development projects that use either Mercurial or Git revision control systems. Sourcetree and Bitbucket integrate closely, are easy to connect, and are a great option when you're starting to use Git.
<h2><span style="font-weight: 400;">Install Sourcetree on Your Mac</span></h2>
The following instructions detail how to download and install the application on your Mac:
<ul>
 	<li>Download the latest version of Sourcetree from the <a href="https://www.sourcetreeapp.com/">official downloads page</a>.</li>
 	<li>Unzip the downloaded zip file by double-clicking it in a Finder window.</li>
 	<li>Copy the extracted <strong>.app file</strong> to the Applications folder.</li>
 	<li>Open the Applications folder in Finder and double-click on the Sourcetree icon.</li>
</ul>
If the steps are all successful, your Sourcetree application should look like the image below:

<img class="aligncenter size-large wp-image-17995" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-18-at-14.40.45-793x1024.png" alt="Sourcetree successful installation" width="793" height="1024" />

This image shows the initial view when opening the Sourcetree application. The application defaults to the <strong>local</strong> tab. On a new install, you'll have no local repositories and the list will be empty.
<h2>Connect to Your Bitbucket Account</h2>
Connecting to your Bitbucket account from Sourcetree will synchronize your source code between your computer and Bitbucket. To connect Sourcetree to Bitbucket, follow these required steps:
<ul>
 	<li>Check for existing remote repositories.</li>
 	<li>Configure the connection.</li>
 	<li>Authorize Sourcetree with Bitbucket.</li>
 	<li>Set up SSH keys.</li>
</ul>
<h3>Check for Existing Remote Repositories</h3>
Select the <strong>Remote </strong>tab in the Sourcetree application to check for any existing repositories connected to accounts.

<img class="aligncenter size-large wp-image-17996" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-18-at-14.44.56-793x1024.png" alt="open remote tab" width="793" height="1024" />

The above view shows the <strong>Remote </strong>tab view without any repositories. A new installation of Sourcetree won't be connected to any accounts. To connect your Bitbucket account, click the <strong>Connect...</strong> button. This will open the Accounts view of the application.
<h3>Configure the Connection</h3>
Click the <strong>Add... </strong>button to open the view shown below.

<img class="aligncenter size-large wp-image-17997" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-18-at-14.58.28-1024x676.png" alt="sourcetree accounts page" width="1024" height="676" />

The above view shows that no account is currently connected and no SSH key is configured. Choose the following for the given drop-down boxes:
<ul>
 	<li><em>Host:</em> We're connecting to a <strong>Bitbucket</strong> host, so leave this value set to the default.</li>
 	<li><em>Auth Type:</em> <strong>OAuth</strong> provides the best security for multiple connections to Bitbucket, so leave this value set to the default.</li>
 	<li><em>Protocol:</em> <strong>SSH</strong> is the most convenient connection because it allows your computer to trust the messages sent to and from Bitbucket. So leave this value set to SSH.</li>
</ul>
<h3>Authorize Sourcetree to Connect to Your Bitbucket Account</h3>
To authorize Sourcetree to connect to your Bitbucket account and provide Sourcetree with an OAuth key for future communications, click the <strong>Connect Account</strong> button. This will open a browser window requesting login details for the Bitbucket website, as shown below.

<img class="aligncenter size-large wp-image-17998" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-18-at-15.07.04-1024x829.png" alt="login to bitbucket" width="1024" height="829" />

The above image shows the login webpage for Bitbucket. Log in to Bitbucket and open the webpage with Sourcetree. The page will redirect back to the Sourcetree application and provide the application with an OAuth token automatically. This allows the Bitbucket to trust any future messages coming from the Sourcetree application.

<img class="aligncenter size-large wp-image-17999" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-18-at-15.13.30-1024x679.png" alt="Added account to sourcetree" width="1024" height="679" />

The view above shows a successfully connected Bitbucket account. The options are as follows:
<ul>
 	<li><em>Bitbucket username:</em> The unique username that you gave on signup. I've chosen "peterjgrainger."</li>
 	<li><em>Bitbucket profile picture:</em> This is an optional profile picture you set up when creating the account.</li>
 	<li><em>Communication protocol:</em> SSH<strong> </strong>is the most convenient and safe way to connect Sourcetree to Bitbucket.</li>
</ul>
There's a warning sign (⚠️) beside the text "SSH." Although we've connected Sourcetree to our Bitbucket account, our computer's SSH key isn't stored in Bitbucket.
<h2>Setting up SSH Keys for Bitbucket</h2>
To enable cloning of a repository locally using Git, create an SSH key for that account. Double-click on the account you've just made and click the <strong>Generate Key</strong> button.

<img class="aligncenter size-large wp-image-18000" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-18-at-15.22.43-1024x678.png" alt="Create an SSH Key" width="1024" height="678" />

The above image shows the "Create an SSH Key" view. To add extra protection to your private SSH key, <a href="https://www.ssh.com/ssh/passphrase">encrypt it with a passphrase</a>. Press the <strong>Create </strong>button to make a private and public SSH key and automatically upload the public key to Bitbucket. Press <strong>Save </strong>and the application will return to the Accounts view.

<img class="aligncenter size-large wp-image-18012" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-18-at-15.30.06-1024x113.png" alt="ssh warning removed from account" width="1024" height="113" />

The Accounts view now no longer shows the warning sign ⚠️ beside <strong>SSH</strong>, and Sourcetree is fully set up.
<h2>Using Sourcetree to Manage Your Bitbucket Repository</h2>
<h3>Create a Private Git Repository on Bitbucket</h3>
A repository is a store for a group of related files. Most people store the source code needed to run an application, but you could store code for multiple applications or a collection of plain text files. How you group your files is up to you. Creating a repository as a private repository allows only you to access and modify the contents.

To create a new private Git repository on Bitbucket, click the <strong>New...</strong> drop-down.

<img class="aligncenter size-full wp-image-18135" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-21-at-17.01.33.png" alt="Create a new remote repository" width="485" height="628" />

The image below shows the options available when creating a new repository. Select <strong>Create Remote Repository </strong>from the list to open the "Create a remote repository" view.

<img class="aligncenter size-large wp-image-18278" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-24-at-12.21.42-1024x565.png" alt="create a private repository" width="1024" height="565" />

The following choices will create a private Git repository named <strong>ASPE-workshop</strong>:
<ul>
 	<li><em>Account:</em> This defaults to your current default account (here, <strong>Bitbucket - peterjgrainger</strong>).</li>
 	<li><em>Owner:</em> This defaults to the user on the default account (<strong>peterjgrainger</strong>).</li>
 	<li><em>Name:</em> Choose a name that describes the files in the repository. This will be converted to lowercase (<strong>ASPE-workshop</strong>).</li>
 	<li><em>Description:</em> This is an optional field that describes what the files are. In the example above, I've left this blank.</li>
 	<li><em>Type:</em> Choose <strong>Git</strong> as the type of repository.</li>
 	<li><em>This is a private repository:</em> Check this value to create a private repository visible only to you and those you invite.</li>
</ul>
Click the green <strong>Create</strong> button to make the remote repository.

You'll see an extra repository in your remote repositories list showing your username, a forward slash (/), and the name you gave your repository. The image below shows the repository I made using the instructions in this blog.

<img class="aligncenter size-full wp-image-18279" src="https://www.hitsubscribe.com/wp-content/uploads/2019/05/Screenshot-2019-05-24-at-12.27.35.png" alt="created repository" width="996" height="94" />
<h2>Use Sourcetree to Manage Your Repositories</h2>
Sourcetree is a great way to start using Git repositories without the steep learning curve of using the command line. Bitbucket integration also makes it easy to connect to an account and enable the creation and administration of repositories directly from the application.

Once you're comfortable with the basics, moving to the command line will be much simpler. You'll already understand the concepts of Git, so the power of the command line will be a lot easier to handle.
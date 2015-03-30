# Introduction

Got Your Back (GYB) is a command line tool that backs up and restores your Gmail account. This page provides simple instructions for downloading, installing and starting to use GYB.

GYB works with Gmail.com and Google Apps accounts.

# Step 1: Download GYB
## Windows Users
Head to the [releases page](https://github.com/jay0lee/got-your-back/releases) and download the latest Windows version of GYB, do not download the Python Source version (unless you really know what you're doing).

## Mac and Linux Users
Head to the [releases page](https://github.com/jay0lee/got-your-back/releases) and download the latest Python source version of GYB, do not download the Windows version.

# Step 2: Extract GYB
## Windows Users
Use the archive extraction tool of your choice to extract the files from the GYB .zip you downloaded. Windows XP and higher have a built in tool that works just fine. When specifying where to extract GYB, I suggest extracting directly to C:\ so that the files will reside in C:\gyb. Once completed, GYB should exist at C:\gyb\gyb.exe

## Mac and Linux Users
Use the archive extraction tool of your choice to extract the files from the GYB .zip you downloaded. I suggest extracting the files to a sub-folder of your home directory.

# Step 3: Running GYB for the First Time
## Windows Users
Open a command prompt on your computer. You can do this by going to Start -> Programs -> Accessories -> Command Prompt or by opening the Run... dialog on the start menu and typing CMD then enter. Now change to the directory where you extracted GAM. The command to change directories looks like:

```
cd \gyb
```

this works if you extracted GYB to c:\gyb. If you extracted it elsewhere, specify that location instead. Now type:

```
gyb --email youremail@gmail.com --action estimate
```

except use your real email address in place of youremail@gmail.com. GYB will open up a web page in order for you to grant access to your Gmail account. This authorization makes it possible for GYB to connect to your Google Account via IMAP and SMTP only, GYB will have no rights to any of your other Google Data. Make sure you are logged in to the Google account you specified before granting access. Once you've granted access, switch back to the command prompt window and hit enter. If no errors are printed, GYB should start estimating the size of your Gmail mailbox. Note that GYB only estimates the size of messages in the All Mail folder, it does not check Spam or Trash although these do count against your Gmail quota displayed at the bottom of your Gmail inbox. To accurately compare GYB's estimate and the Gmail inbox web page quota display, first empty your Trash and Spam folders.

Congratulations, you're up and running with GYB! You probably want to move on to [performing a backup](https://github.com/jay0lee/got-your-back/wiki#step-4-performing-a-backup) now.

## Mac and Linux Users
Open up a terminal window on your computer. On Linux, this is generally under Accessories -> Terminal. On Mac, it's under Applications -> Utilities -> Terminal. Now change to the directory where you extracted GYB. Try:

```
cd ~/gyb
```

this will work if you extracted the GYB files to a subfolder named gyb in your home directory. If you extracted them elsewhere, replace ~/gyb with the full path to them. Now run:

```
python gyb.py --email youremail@gmail.com --action estimate
```

If you get an error about python not being a valid program, make sure you have the Python interpreter installed on your machine. All Macs and most Linux installs should include Python but if not, you may need to research how to install it on your OS/Distribution.

GYB will open up a web page in order for you to grant GYB access to your Gmail account. This authorization makes it possible for GYB to connect to your Google Account via IMAP and SMTP only, GYB will have no rights to any of your other Google Data. Make sure you are logged in to the Google account you specified before granting access. Once you've granted access, switch back to the command prompt window and hit enter. If no errors are printed, GYB should start estimating the size of your Gmail mailbox. Note that GYB only estimates the size of messages in the All Mail folder, it does not check Spam or Trash although these do count against your Gmail quota displayed at the bottom of your Gmail inbox. To accurately compare GYB's estimate and the Gmail inbox web page quota display, first empty your Trash and Spam folders.

Instead of needing to type "python gyb.py" for every command, you can mark the gyb.py file as executable or we can use the alias command to shorten it to just "gyb":

```
alias gyb="python gyb.py"
```

Now when we can just type commands like:

```
gyb --email myemail@gmail.com --action estimate
```

you'll need to type the alias command each time you open a Terminal to run GYB. 

Congratulations, you're up and running with GYB! You probably want to move on to [performing a backup](https://github.com/jay0lee/got-your-back/wiki#step-4-performing-a-backup) now.

# Step 4: Performing A Backup
A basic GYB backup is very easy to start. Just run:

```
gyb --email youremail@gmail.com --action backup
```

the "--action backup" is not strictly necessary since GYB defaults to backing up if an action is not specified. Assuming you've already granted GYB access to your Gmail messages, GYB will load the access token from youremail@gmail.com.cfg and use it to get access to your messages. By default, GYB will download and save all messages to a folder named "GYB-GMail-Backup-youremail@gmail.com". You can specify another folder for GYB to use with the --local-folder argument:

```
gyb --email youremail@gmail.com --local-folder "C:\Users\John\Documents\Johns_Gmail_Backup"
```

GYB will keep you update you as the backup progresses.

# Step 5: Performing a Restore
Restores on GYB are also very simple:

```
gyb --email youremail@gmail.com --action restore --local-folder "c:\my_gmail_backup"
```

the specified folder should exist and should have been used in a previous GYB backup. If not specified, restores default to using the GYB-GMail-Backup-youremail@gmail.com folder just like the backup does. GYB will connect to your Gmail account and perform the restore of all messages in the backup folder.

Note that if you perform a restore to the same Gmail account, GYB will not create duplicate messages, instead you'll only see messages restored which were backed up by GYB then later deleted from the Gmail account.

If you want to restore messages to an account other than the one you backed up, it's necessary to specify the backup folder. As an example,

```
gyb --email newaddress@gmail.com --action restore --local-folder GYB-GMail-Backup-oldaddress@gmail.com
```

will look for messages in the backup of the oldaddress@gmail.com account but restore them to the newaddress@gmail.com account.

You can also use the "--label-restored NEWLABELNAME" argument to set a label on all restored messages. For example:

```
gyb --email newaddress@gmail.com --action restore --local-folder GYB-GMail-Backup-oldaddress@gmail.com --label-restored "Old Address"
```

will restore the message, always including an extra label of "Old Mail" on the restored messages.

# Selective Backups With Gmail Searching
GYB supports selective backups using Gmail style mailbox searches. For example, suppose you wanted to only backup important or starred messages:

```
gyb --email youremail@gmail.com --search "is:important OR is:starred"
```

would cause GYB to only backup messages matching that search query. Virtually any Gmail search will work with GYB. The only exception being that specifying "in:anywhere" will not backup Trash and Spam, there's currently no way to backup Trash and Spam with GYB. [http://mail.google.com/support/bin/answer.py?answer=7190 See here for a detailed article] on all of the possible Gmail Search parameters.

Note that Gmail searches also work with the "--action estimate" command. Suppose you wanted to know how much space emails with .PDF attachments are using in your Gmail mailbox:

```
gyb --email youremail@gmail.com --action estimate --search "filename:PDF"
```

will estimate the size of messages with PDF attachments only. Try substituting DOC, JPG, ZIP and other common file attachments for PDF.

# Advanced options

## --search
Optional: On backup, estimate, count and purge, Gmail search to scope operation against.

## --local-folder
On backup, restore, estimate, local folder to use. Default is GYB-GMail-Backup-<email>

## --label-restored
On restore, all messages will additionally receive this label. For example, "--label_restored gyb-restored" will label all uploaded messages with a gyb-restored label.

## --strip-labels
On restore and restore-mbox, strip existing labels from messages except for those explicitly declared with the --label-restored parameter.

## --noresume
GYB keeps a record of messages restored to each account and will pick up where it left off should the restore not finish. The <code>--noresume</code> switch will make GYB ignore messages already restored and restart the restore at the beginning.

## --fast-incremental
By default, GYB will refresh the stored labels and flags for messages that have already been backed up, just in case they changed after the backup. This step can be skipped by supplying the ```--fast-incremental``` switch on the command line.

## --action reindex
GYB keeps track of the messages backed up by using the IMAP UID values for the messages. It does not happen often, but it is sometimes necessary to reorganize an IMAP mailbox and assign new UIDs. When this happens, GYB will refuse to use the existing folder for further backups. The ```--action reindex``` action will rebuild the UID index using information from the message headers. It is slow, but faster than re-downloading the full account to start a new backup.

(Another option is to make a new backup folder, but include a search for "after:" some recent date for that folder. If it is ever necessary to restore the mailbox, you would then need to restore from both folders.)

## --action restore-mbox
Restore mbox files that you've exported from [Gmail Takeout](https://www.google.com/settings/takeout), [Google Vault](https://support.google.com/vault/answer/2462365?hl=en), [GAM Email Audit Exports](https://github.com/jay0lee/GAM/wiki/ExamplesAccountAuditing#user-mailbox-exports) or any other MBOX format file you have.

Use the --local-folder option to specify the path where you've extracted all of your mbox files. GYB will restore messages from all .mbox and .mbx files in the directory and any sub-directories.

## --action restore-group
Google Apps for Business and Education Only. This feature allows you to restore messages to a Google Group rather than a user mailbox. It's important to note that:
 * Message labels, read/unread status, stars and other metadata are not preserved with restore-group.
 * There is no API or method to extract or backup messages stored in Google Groups. GYB can restore messages to a group but cannot backup message in a group, it's a one-way process.
 * The Groups Migration API supports a maximum message size of 16mb so not all Gmail-stored messages can be imported into a group.
 * Groups have no quota! If you're okay with the above issues, you can offload an unlimited amount of data to a group. This may be a good solution for users approaching their 25gb quota in Gmail.

This option requires the --use-admin option below be specified. The --email option should be the Google Group to restore messages into. Archiving for the group should be enabled.

A good use case for restore-group would be a user who is nearing Gmail quota. You could do a selective backup of the user's mailbox with a GYB backup using <code>--search before:2011/04/13 smaller:16M</code> to get only messages older than 2 years and smaller than 16mb. Then restore the messages to a Google Group and give the user exclusive access to the new group. Finally, free up the user's mailbox by performing a purge using the same search parameters. I'd also recommend holding on to the local backup of the user's mail should you ever wish to restore to the mailbox.

## --use-admin
Specify the Google Apps admin to utilize when restoring messages to a group with --action restore-group. This user should be a super administrator, delegated admins do not have sufficient privileges to perform group restores.

## --action count
Just count the number of messages in a user mailbox. Note, to compare this number to what you see in Gmail, you should turn conversation mode off in general settings and search for "-is:chat". This ensures you are counting individual messages (not conversations) and that archived chats which are not backed up by GYB by default are not counted.

## --action purge
DANGEROUS!!! This option will completely delete messages. Running this command without a <code>--search</code> parameter will EMPTY YOUR ENTIRE MAILBOX. This removes messages from Trash and Spam folders also so there is no ability to restore from the mailbox itself. Use this option with extreme caution. You have been warned. It is highly recommended to do a backup before a purge.

## --action purge-labels
DANGEROUS!!! This option will delete all user labels for the mailbox. No messages will be deleted but everything will be left unlabeled!

## --use-imap-folder
By default, GYB always connects to the "All Mail" IMAP folder in order to perform backups, estimates, restores and counts. However, with this option, you can specify another IMAP folder to utilize. This option is most useful in backing up Archived Chats by specifying <code>--use-imap-folder [Gmail]/Chats</code>. Make sure the Chats folder is visible to IMAP in your Gmail Settings -> Labels before running this. Note that chats cannot be restored to the Chats label in Gmail since the folder is read-only for IMAP.

## --batch-size
By default, GYB grabs the full content of 100 messages at a time for backup. If the mailbox has lots of very small messages, you may see better backup performance by backing up more messages at once with a --batch-size 500 parameter. If the mailbox has many very large messages, it may take a very long time for GYB to backup anything as it could be pulling down up to 2500mb (100 messages x 25mb each) of data for each batch. Try specifying something smaller like --batch-size 4.

## --service-account
Use a Google Service Account to authenticate rather than standard 3-legged OAuth authentication. This option is only for Google Apps for Business and Education users. See below for details.

# Google Apps Business and Education Admins: Backup, Restore and Estimate Users and Restore to Groups
If you're using Google Apps for Business or Education Edition, it's possible to use GYB with your users without needing to know their password. This works because GYB makes use of a special Google Apps feature called Service Accounts.

There are a few steps involved with creating and authorizing a service account for GYB.

1. Go to the [Google Cloud Console](https://cloud.google.com/console)
1. Click "Create Project"
1. Give your project a name and a unique project ID
1. Once the project is finished being created, click "APIs & auth" to the left
1. Go to the "APIs" menu on the left
1. In the list of APIs, scroll down to "Groups Migration API" and click the switch to toggle it ON. Agree to the terms.
1. Go to "Credentials" menu to the left.
1. Click the blue "CREATE NEW CLIENT ID" button.
1. Choose the "Service Account" radio button and click the blue "Create Client ID" button.
1. Your browser will download a file. Change the downloaded file name to privatekey.p12 and save it to the same location as gyb.exe or gyb.py
1. Click the blue "Okay, got it" button.
1. Under the ***Service Account section***, make a note of the Client ID and Email Address Values. You'll need them later so either copy them into Notepad or keep the API console window open in another tab. Make sure you're using the Service Account values, not the ones under "Compute Engine and App Engine"
1. Go to your Google Apps Control Panel (https://admin.google.com )
1. Click Security
1. Click Show More
1. Click Advanced Settings (Managed advanced security features such as authentication and integrating Google Apps with internal services).
1. Click Manage API Client Access
1. At this point, you are at (unless Google has changed URLs again) https://admin.google.com/AdminHome?chromeless=1#OGX:ManageOauthClients
1. For Client Name, enter the Client ID you recorded above. For API Scopes, enter exactly: 

```
https://mail.google.com/,https://www.googleapis.com/auth/apps.groups.migration
```

Now you can run GYB with the service account option. Specify your service account email address from above when using --service-account.

Try running:

```
gyb --email yourusersemail@yourcompany.com --service-account 123456789@developer.gserviceaccount.com
```

in this example, replace 123456789@developer.gserviceaccount.com with your service account email address from the cloud console.

WARNING: Service Accounts offer very powerful control over your Google Apps domain. Do not use this option on a computer you do not trust! Do not leave the privatekey.p12 file in places where others can find it! If you suspect your Service Account has been stolen, delete the API project in the API console and unauthorize it's access to your domain.

# Troubleshooting

Below you'll find some possible errors and what they could mean.

## invalid_grant

This can occur if you have a typo in your service account command line, e.g. you have the client id instead of the email address.

## access_denied

This can occur if you've failed to add the proper API scopes (mail, groups migration) for a service account.

## invalid_request

This can occur if you're attempting to download mail from an account that is suspended; only active accounts can be backed up via Got Your Back.
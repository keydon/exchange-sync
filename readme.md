Overview
========
Exchange Sync will read data from your Exchange account and export flagged emails (not Exchange tasks) to Remember The Milk, and calendar appointments to Google Calendar. It will not modify data in your Exchange account in any way. Google has a similar service for Windows users called Google Calendar Sync. This application can be considered Google Calendar Sync for Linux and Mac (but will also work on Windows), with the additional Remember The Milk functionality.

Linux Usage Instructions
========================
1. Create a folder in your home directory called exchange-sync.
2. Download the [release jar](https://github.com/gdenning/exchange-sync/releases/download/1.0.8/exchangesync-1.0.8-jar-with-dependencies.jar) to ~/exchange-sync.
3. Download the [sample properties file](https://github.com/gdenning/exchange-sync/releases/download/1.0.8/exchangesync.properties) to the same folder.
4. Modify exchangesync.properties file as follows:
    - Set exchangeHost to the hostname you usually use to access Outlook Web Access.
    - Set exchangeDomain, exchangeUsername, and exchangePassword to your Microsoft Exchange domain, username, and password.
    - Set rtmListName to the name of the Remember the Milk list that you want to export tasks to.
    - Set googleCalendarName to the name of the Google Calendar that you want to export appointments to. This should match one of the calendar names under the "My Calendars" list on the left-hand side of your Google Calendar view.
5. Create a symlink to the jar to simplify upgrades: <code>ln -s ~/exchange-sync/exchangesync-1.0.6-SNAPSHOT-jar-with-dependencies.jar ~/exchange-sync/exchangesync.jar</code>
6. Execute <code>env EDITOR=nano crontab -e</code> and add the following line: <code>*/30	*	*	*	*	java -jar ~/exchange-sync/exchangesync.jar > ~/exchange-sync/exchangesync.log 2>&1</code>

exchangesync.properties Values
==============================

Setting | Example | Description
------- | ------- | -----------
syncTasks | true | Determines whether flagged Exchange emails will be exported to Remember the Milk.
syncAppointments | true | Determines whether Exchange calendar appointments will be exported to Google Calendar.
exchangeHost | mail.sample.com | The host name of your Exchange mail host.
exchangeUsername | username | Your Exchange username.
exchangeDomain | COMPANY | Your Exchange domain.
exchangePassword | password | Your Exchange password.
exchangeVersion | Exchange2010_SP1 | Your Exchange version. Must be one of: Exchange2007_SP1, Exchange2010, Exchange2010_SP1. Leave blank if not sure or if you have a version newer than Exchange 2010 SP1.
rtmListName | Inbox | Name of the Remember The Milk list where you want tasks to be created.
googleCalendarName | Work Calendar | Name of the Google calendar where you want appointments to be created.
usingProxy | false | Determines whether Internet access is via an HTTP proxy.
proxyHost | 192.168.1.2 | IP address or host name of the HTTP proxy server.
proxyPort | 3128 | Port number of the HTTP proxy server.
obfuscateAttendeeEmails | true | Determines whether organizer and attendees on Google Calendar appointments should be obfuscated to prevent Google from emailing those users when calendar appointments change.
googleSyncOrganizerAndAttendees | true | Determines whether organizer and attendees should be populated on Google Calendar appointments. Some users have reported that this causes a "Calendar Usage Limits Exceeded" message.
appointmentMonthsToExport | 1 | Number of months in the future to export appointments for.

Developer Instructions
======================
1. Install Maven and Git: <code>sudo apt-get install maven git</code>
2. Change to your home folder: <code>cd ~</code>
3. Download the source: <code>git clone https://github.com/gdenning/exchange-sync.git</code>
4. Change to the exchange-sync folder: <code>cd exchange-sync</code>
5. Modify exchangesync.properties as follows:
    - Set exchangeHost to the hostname you usually use to access Outlook Web Access.
    - Set exchangeDomain, exchangeUsername, exchangePassword to your Microsoft Exchange domain, username, and password.
    - Set rtmListName to the name of the Remember the Milk list that you want to export tasks to.
    - Set googleCalendarName to the name of the Google Calendar that you want to export appointments to. This should match one of the calendar names under the "My Calendars" list on the left-hand side of your Google Calendar view.
6. Copy exchangesync.properties to your home folder: <code>cp exchangesync.properties ~/exchangesync.properties</code>
7. Compile the application: <code>mvn install</code>
8. Create a symlink to the application in your home directory: <code>ln -s ~/exchange-sync/target/exchangesync-1.0.6-jar-with-dependencies.jar ~/exchangesync.jar</code>
9. Execute <code>env EDITOR=nano crontab -e</code> and add the following line: <code>*/30	*	*	*	*	java -jar ~/exchange-sync/exchangesync.jar > ~/exchange-sync/exchangesync.log 2>&1</code>

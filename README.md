# Gmail-auto-delete-by-sender-s-domain

************* END USER:
Click a spam email that escaped Google's filters. Instead of using Gmail's "move to spam" or "block sender" buttons, 
you can use this tool to block the sender's entire domain for the future.

All you need to do is type CTRL-L to add a special label to the thread. When this script runs (on a time trigger), 
it will find those tagged emails, and add the senders whole domain to a Filter that sends their emails to the trash bin.


************* HOW TO DEPLOY THIS SCRIPT:
1. Manually set up a filter in your gmail account.  The FROM address in the filter MUST be "autoblock@autoblock.com".  The actions on the filter can 
be whatever you want:
- Delete
- Add a label
- Archive
- Move to Spam
- Etc.

2. Manually create two labels: "_block_domain" and "_domain_blocked".  (If you don't like those, you can use something else, but make sure to edit the Constants 
in the top of the code.

3. Copy the code into a GAS script file and save

4. Set up a Trigger in your project to run the function blocksenderDomains() periodically.  (Mine is set to every 2 hours; how often isn't very important.)

************* HOW THE SCRIPT WORKS:
-The blocksenderDomains() function searches for all email threads with the "target" label ("_block_domain").
-It identifies the Sender for each message, and extracts the sender's domain from the address.
-It concatenates the domains into a string "tempDomains" and passes that through to the other function, updateAutoblockFilter()

- updateAutoblockFilter looks in your Script Properties to find the ID of the correct Gmail filter.
- If it can't find the filter using that ID (or if the ID doesn't exist yet), then it retrieves ALL of your filters and loops through them, 
looking for the one that has the FROM property with "autoblock@autoblock.com" address in it.
- If it still can't find the filter, it errors out and tells you to set up the filter and try again.
- When it finds the filter, it extracts the FROM line.
- Then it adds the new domains to the FROM line.
- Then it creates a new (identical) filter, with the new FROM line. (Why? Because the Gmail API does not provide a function to update/edit a filter, 
only to Create and Remove them)
- Then it deletes the old filter.


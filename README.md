RULA BookFinder
===========

Many generations of users have wandered through library buildings and stacks, bewildered and lost, when looking for a book or a physical item. The RULA BookFinder application aims to help library users find their way through maps and enhanced floor plans without needing to rely on the LC call number ranges on the shelves.

Setup
-----

Create a database, and run INSTALL_SQL.sql to create tables and default user account.

Modify the following files:

    config/database.php        
        Lines 44-47 
            - Set the MYSQL hostname/username/password/database

    config/constants.php    
        Lines 49-53 
            - Set the Summon API Keys (search will not work without these)
            - Set the admin email address (notifications will be sent to this address if an item cannot be locatated to a shelf)

    config/config.php        
        Line 227 
            -Set the encryption key

    config/email.php        
        Line 17 
            - Set the base url (optional)
        Line 32 
            - Set values for SMTP Server

    models/model_api.php
        - Function "catalogue_info" contains hardcoded values designed to work with Ryerson's implementation of Millennium.
          It currently uses screenscraping, so the pre-made regular expressions may not work for your implementation.
        
        **Note**: We encourage the community to produce a Z39.50 version of this function so it can be more easily/widely adpoted by libraries with less technical staff.

Usage
-----

1. Log in to the admin panel ( <install url>/admin ). The default username/password will be admin/admin.
2. Create a building under the "Manage Buildings" link. Be aware that while the backend is structured to support multiple buildings, it is not fully implemented
3. Create floors in the building using the "Manage Floors" option. Currently, the system only supports images (no SVG's yet). PNG is recommended. The "weight" refers to the ordering of the floors. Floors will be sorted based on this (larger weight appears closer to the bottom of sorting)
4. Create "Item Types". Items labelled as a stack will be based searchable based on their LC Call Number. Displaying non-stack items has not yet been fully implemented
5. Create "Stack Types". The "Catalogue Pattern" is based on MYSQL syntax for matching. For example, if you have oversize items in your catalogue labelled like "Oversize (5th Floor)" or "Oversize (6th Floor)" then your pattern should be "Oversize%". In the event where an item is located in multiple locations on the same record (located on both Reserve and Stacks for example), a priority can be set to determine which result is displayed first
6. Map LC Call numbers to shelves using "Modify Stacks". Click and drag rectangles over the stacks on your floor image, using the tabs to switch floors. Assign an Call No Start and Call No End to identity which items are on that shelf. Assign the stack type for that item. To Edit items, simply click the box you want to edit, fill in the new values, and save.

If all is working properly, you should now be able to map your items to the shelves. This can be done by using the integrated search (Summon required), or by entering the URL ( <install url>/#s=<record number>) for your desired item.
For example, http://apps.library.ryerson.ca/bookfinder/#s=b2180373

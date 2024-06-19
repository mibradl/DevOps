# Chapter6
This module we review clean up policies

# Name: Clean-up Policies

# Description: 

Clean up policies are used to clean up old artifacts no longer needed.

From the top select Repository

    Seclect Clean up Policies

    Create Clean up Policy button

        Name: maven

        Format: maven2

        Clean up criteria

            Component Age: 30   days ago

            Component Usage: 20     days

            Release Type:  Pre-Release / Snapshot Version

            Asset Name Matcher: regex pattern (.*)

                Results may only be a sample of what will be deleted using the current criteria.

                Component count (matching criteria) viewing 2 out of 2.



        To set the clean-up policy

            Select Repository

            Click Name of Repository: maven-snapshot

                Scroll to the bottom of page for clean-up policy options

                Select Components from available filter to be deleted: maven

                Click right arrow to move to Applied

                Save


    Configuring on Clean-up Policy

        Clean up schedule can found under System >> Tasks


                        ID	1bbd7d1e-5e3b-4138-b72a-20bafee8e331
            Name	Cleanup service
            Type	Admin - Cleanup repositories using their associated policies
            Status	Waiting
            Next run	Wed Jun 19 2024 21:00:00 GMT-0400 (Eastern Daylight Time)
            Last run	Tue Jun 18 2024 21:00:00 GMT-0400 (Eastern Daylight Time)
            Last result	Ok [0s]

        Once a day the clean up tasks will be enforced


        From Task page click Create Task to perform specific task manually

        Selecting an item to be delete does not physically remove item.  This is called a soft delete.

        To remove an item physically requires a compaction of the blob store

            Create Task

            Create Admin  - Compact blob store Task

            Select Default store

            Task Frequency: Daily

            Start Date: tomorrow

            Time of Task: 03:00

            Save



Manuall execute both tasks

        Run Cleanup service (Admin - Cleanup repositories using their associated policies) task? Click Yes to confirm

        Last Result:  ok


Go back Repository >> Browse >> Maven-snapshots

        Previous Jars have been removed, but this doesn't physicall remove anything.


        Blobs Stores - still shows files are allocated

        Still need to run admin - compact blob store

        Run this manually

        Last Result: ok

        Default Repository only has a Blob count of 1.

        /opt/sonatype-work/nexus3/blobs/default/content - contents are now empty.



                

# Usage


    
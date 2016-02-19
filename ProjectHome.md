Connects to gmetad and, using a rules-based config file, sends alerts to a specified email address.   Simple PERL implementation.

Daemon properly starts, stops, HUP's, etc., rules can be host-specific, alarm reports via are reasonably condensed (with alarm counts, etc.).

Most of the options are in the ConfigFile. But more up-to-date documentation should be obtained via "ganglia-alert -h".

[Browse Code](http://code.google.com/p/ganglia-alert/source/browse/trunk)
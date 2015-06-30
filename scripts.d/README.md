DESCRIPTION
-----------

This directory contains bootstrap stages to be executed by initialize_instance.
Scripts are numbered to control the order they are executed in (think sysvinit
styles rc.d/ directories).  You may place additional bootstrapping scripts in
your project-config repository

For numbering your own scripts there are two rules: 

* Numbers must be written in three-digit format (e.g. '015' instead of '15')

* Multiples of 20, including '000' (e.g. '000', '020', '040') are reserved for
  Syseleven's use. Apart from that anything goes (just pick a number that will
  insert your own script between the desired Syseleven scripts.

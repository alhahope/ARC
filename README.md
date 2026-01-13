The code is reprinted from https://bitbucket.org/uw-madison-networking-research/arc/src/master/

ABSTRACT REPRESENTATION FOR CONTROL PLANES (ARC)

Building ARC
============
Prerequisites
-------------
- ant
- Batfish (http://www.batfish.org): a git repository forked from the 
  Batfish code repository is included as a submodule of this git repository; 
  run `git submodule update --init` to clone the forked Batfish code repository 
  into the `libs/batfish` directory
- graphviz: only required for rendering graphs output by ARC 
- Gurobi (version 6.5 or newer): only required for equivalence testing;
  academic users can obtain a free single-user license
- Java JDK (version 8 or newer)

Compilation
-----------
1. Clone the ARC git repository

        git clone https://bitbucket.org/uw-madison-networking-research/arc.git

2. Obtain the Batifish source code

        cd arc
        git submodule update --init

3. (Optional) Copy or symlink the `gurobi.jar` file from the lib directory of your Gurobi installation to the `projects/arc/lib` directory

4. Compile

        cd projects/arc/
        ant

The compiled program is loated in the `target/` directory (within `projects/arc/`); all requisite libraries are copied into a `lib/` subdirectory within the `target/` directory.

Using ARC
=========
Prerequisites
-------------
Create a directory containing a snapshot of the configurations from all routers in the network whose control plane you want to analyze. Example configurations for toy networks are included in the configs/examples directory. ARC currently only supports configurations from Cisco devices.

Basic Usage
-----------
Run ARC (from the `projects/arc/target` directory) using the following command:

    java -cp *:lib/* edu.wisc.cs.arc.Driver -configs <CONFIGS-DIR>

`CONFIGS-DIR` is the path to a directory containing the configuration files for all devices in the network whose ARC should be generated/analyzed.

To produce visual representations of the ETGs contained in an network's ARC, as well as the network's routing instances and (inferred) physical topology, include the `-graphs <GRAPH-DIR>` option. With this option, ARC will output graph files that be rendered using graphviz. You can use the following command to render all graphs produced by ARC: 

    projects/arc/scripts/render_graphs.sh <GRAPH-DIR>

To only render the base ETG (i.e., an ETG from which per-traffic-class ETGs are derived), instance graph, and physical topology graph, include the `-b` argument before the GRAPH-DIR when running the script.

To verify a particular invariant, include the appropriate `-v` argument (e.g., `-vab` to verify always blocked) when running ARC. To run all verifiers, include the `-vall` argument.

To see all options for ARC, use the `-help` argument:

    java -cp *:lib/* edu.wisc.cs.arc.Driver -help

The `projects/arc/scripts/` directory contains a variety of scripts that can aid in running ARC over many networks and parsing ARC's output.

Generating FAT Tree Topology
=========
Basic Usage
-----------
1. Compile

        cd projects/fattree/
        ant

2. Run FAT Tree generation code (from the `projects/fattree/target` directory) using the following command:

        java -cp *:lib/* CreateFATTreeTopology -help

The `-help` argument shows all options for creating FAT Tree topology.

## Nothing to see yet

I was toying with this idea for a while, and now that we successfully tested it on py-osrm, I'd like to implement it.

The need for this was arising from building Python bindings for both projects, where we have to install/build all dependendencies of the project, plus the project itself, before we can compile the binding code.

The idea for this repo would be to have a series of GHA workflow which:
- have a `workflow_dispatch` event so they can be run on button push and a scheduler set to 90 days (Github artifact retention period)
- are based on an "old" linux (e.g. `manylinux2014` aka CentOS7) so we get compatibility with a wide range of linux distros
- build the project's dependencies from source for each platform (linux/win/macos), so we're more flexible with versions and it's easier to package the artifacts
- build the actual project on all platforms from those dependencies
- upload a worklow's artifact to Github

Dependencies and OSRM/Valhalla should be available from different workflow artifacts. The reason being that dependencies rarely need to change, but likely we want to update OSRM/Valhalla when there's a new release, which happens more regularly.

That way consuming projects could simply pull the binary artifacts prior to their own project's compilation.

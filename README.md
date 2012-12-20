startd_cron
===========

Useful HTCondor startd cron scripts.

===========

check_cvmfs - test the health of a FUSE-mounted cvmfs repository and
              advertise information about it

Example of how to use check_cvmfs:

Insert the following into the condor configuration.  Change UWCMS to a
name suitable for your situation.  Replace the path to check_cvmfs
with the actual one.  Also replace the repository to test in the ARGS
with the actual one.

STARTD_CRON_JOBLIST = $(STARTD_CRON_JOBLIST) CHECK_UWCMS_CVMFS
STARTD_CRON_CHECK_UWCMS_CVMFS_PREFIX = UWCMS_CVMFS_
STARTD_CRON_CHECK_UWCMS_CVMFS_EXECUTABLE = /opt/hawkeye/check_cvmfs
STARTD_CRON_CHECK_UWCMS_CVMFS_PERIOD = 10m
STARTD_CRON_CHECK_UWCMS_CVMFS_MODE = periodic
STARTD_CRON_CHECK_UWCMS_CVMFS_RECONFIG = false
STARTD_CRON_CHECK_UWCMS_CVMFS_KILL = true
STARTD_CRON_CHECK_UWCMS_CVMFS_ARGS = cms.hep.wisc.edu

Jobs that require CVMFS should be submitted with the following:

Requirements = TARGET.UWCMS_CVMFS_Exists == True

In addition, it may be desirable to require that the CVMFS catalog
version on the execute node be at least as new as the one on the
submit node.  To do this, insert requirements into the job.

Example shell script snippet that generates requirements to be
inserted into submit file:

local_cvmfs_revision=`attr -q -g revision /cvmfs/cms.hep.wisc.edu`
if [ "$local_cvmfs_revision" != "" ]; then
  REQUIREMENTS="${REQUIREMENTS} && TARGET.UWCMS_CVMFS_Revision >= ${local_cvmfs_revision}"
fi


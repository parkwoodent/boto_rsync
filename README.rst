================================================================================
boto rsync v0.5
================================================================================

Copyright (c) 2011 Seth Davis    
http://github.com/seedifferently/boto_rsync


Synopsis
================================================================================

boto-rsync is a rough adaptation of boto's s3put script which has been
reengineered to more closely mimic rsync. Its goal is to provide a familiar
rsync-like wrapper for boto's S3 and Google Storage interfaces.

By default, the script works recursively and differences between files are
checked by comparing file sizes (e.g. rsync's --recursive and --size-only
options). If the file exists on the destination but its size differs from
the source, then it will be overwritten (unless the -w option is used).


Installation
================================================================================

To install, simply::

    pip install boto_rsync

or::

    easy_install boto_rsync

* You'll need to have Python 2.5+ and either pip or setuptools installed.
* pip/easy_install should automatically install boto for you, but the advanced
  user can find it here: http://github.com/boto/boto/


Usage
================================================================================

::

    boto-rsync [OPTIONS] SOURCE DESTINATION

SOURCE and DESTINATION can either be a local path to a directory or specific
file, a custom S3 or GS URI to a directory or specific key in the format of
s3://bucketname/path/or/key, a S3 to S3 transfer using two S3 URIs, or
a GS to GS transfer using two GS URIs.


Examples
================================================================================

::

    boto-rsync [OPTIONS] /local/path/ s3://bucketname/remote/path/

or::

    boto-rsync [OPTIONS] gs://bucketname/remote/path/or/key /local/path/

or::

    boto-rsync [OPTIONS] s3://bucketname/ s3://another_bucket/


Options
================================================================================

::

    -a/--access_key <key>       Your Access Key ID. If not supplied, boto will
                                use the value of the environment variable
                                AWS_ACCESS_KEY_ID (for S3) or GS_ACCESS_KEY_ID
                                (for GS).
    -s/--secret_key <secret>    Your Secret Access Key. If not supplied, boto
                                will use the value of the environment variable
                                AWS_ACCESS_KEY_ID (for S3) or GS_ACCESS_KEY_ID
                                (for GS).
    -d/--debug <debug_level>    0 means no debug output (default), 1 means
                                normal debug output from boto, and 2 means boto
                                debug output plus request/response output from
                                httplib.
    -r/--reduced                Enable reduced redundancy on files copied to S3.
    -g/--grant <policy>         A canned ACL policy that will be granted on each
                                file transferred to S3/GS. The value provided
                                must be one of the "canned" ACL policies
                                supported by S3/GS: private, public-read,
                                public-read-write (S3 only), or
                                authenticated-read
    -w/--no_overwrite           No files will be overwritten, if the file/key
                                exists on the destination it will be kept. Note
                                that this is not a sync--even if the file has
                                been updated on the source it will not be
                                updated on the destination.
    --ignore_empty              Ignore empty (0-byte) keys/files/directories.
                                This will skip the transferring of empty
                                directories and keys/files whose size is 0.
                                Warning: S3/GS often uses empty keys with
                                special trailing characters to specify
                                directories.
    -p/--preserve_acl           Copy the ACL from the source key to the
                                destination key (only applies in S3/GS to S3/GS
                                transfer mode).
    -e/--encrypt_keys           Enable server-side encryption on files copied
                                to S3 (only applies when S3 is the destination).
    --delete                    Delete extraneous files from destination dirs
                                after the transfer has finished (e.g. rsync's
                                --delete-after).
    -n/--no_op                  No files will be transferred, but informational
                                messages will be printed about what would happen
                                (e.g. rsync's --dry-run).
    -v/--verbose                Print additional informational messages.


Known Issues and Limitations
================================================================================

* Due to the nature of how directories work in S3/GS, some non-standard folder
  structures might not transfer correctly. Empty directories may also be
  overlooked in some cases. When in doubt, use "-n" first.
* Differences between keys/files are assumed _only_ by checking the size.
* Python 3.x support has not yet been tested.
* At this time, the script does not take advantage of boto's "multipart"
  transfer methods. (pull requests welcome!)
* As of this writing, the release version of boto (2.1.1) seems to be buggy
  when attempting to perform GS to GS transfers. Use the latest boto github
  source if you need this functionality.


Disclaimers and Warnings
================================================================================

This is Alpha software--always remember to use the "-n" option first!

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHOR BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF
CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
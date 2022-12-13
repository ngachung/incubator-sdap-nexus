To create a release candidate, you will need:

* Apache LDAP credential for SVN
* A GPG key for signing, published in `KEYS <https://dist.apache.org/repos/dist/release/incubator/sdap/KEYS>`_

Instructions for creating key: https://infra.apache.org/openpgp.html#generate-key

Use :code:`gpg2 --full-gen-key`

Instructions for adding public keys to LDAP: https://home.apache.org/keys/

Set working directory

.. code-block:: bash

  export RELEASE_WORKING_DIR=/Users/sdap/release/

Tag and package Nexus Proto source

.. code-block:: bash

  cd $RELEASE_WORKING_DIR
  git clone --branch release/1.0.0 https://github.com/apache/incubator-sdap-nexusproto.git
  cd incubator-sdap-nexusproto/
  git tag -a 1.0.0-rc2 -m "Release 1.0.0-rc2"
  git push --tags
  git ls-files > /tmp/manifest.txt
  tar cvfz apache-sdap-nexusproto-1.0.0-src.tar.gz -T /tmp/manifest.txt
  gpg --armor --output apache-sdap-nexusproto-1.0.0-src.tar.gz.asc --detach-sig apache-sdap-nexusproto-1.0.0-src.tar.gz
  shasum -a 512 apache-sdap-nexusproto-1.0.0-src.tar.gz > apache-sdap-nexusproto-1.0.0-src.tar.gz.sha512

Tag and package Ingester source

.. code-block:: bash

  cd $RELEASE_WORKING_DIR
  git clone --branch release/1.0.0 https://github.com/apache/incubator-sdap-ingester.git
  cd incubator-sdap-ingester/
  git tag -a 1.0.0-rc1 -m "Release 1.0.0-rc1"
  git push --tags
  git ls-files > /tmp/manifest.txt
  tar cvfz apache-sdap-ingester-1.0.0-src.tar.gz -T /tmp/manifest.txt
  gpg --armor --output apache-sdap-ingester-1.0.0-src.tar.gz.asc --detach-sig apache-sdap-ingester-1.0.0-src.tar.gz
  shasum -a 512 apache-sdap-ingester-1.0.0-src.tar.gz > apache-sdap-ingester-1.0.0-src.tar.gz.sha512

Tag and package Nexus source

.. code-block:: bash

  cd $RELEASE_WORKING_DIR
  git clone --branch release/1.0.0 https://github.com/apache/incubator-sdap-nexus.git
  cd incubator-sdap-nexus/
  git tag -a 1.0.0-rc1 -m "Release 1.0.0-rc1"
  git push --tags
  git ls-files > /tmp/manifest.txt
  tar cvfz apache-sdap-nexus-1.0.0-src.tar.gz -T /tmp/manifest.txt
  gpg --armor --output apache-sdap-nexus-1.0.0-src.tar.gz.asc --detach-sig apache-sdap-nexus-1.0.0-src.tar.gz
  shasum -a 512 apache-sdap-nexus-1.0.0-src.tar.gz > apache-sdap-nexus-1.0.0-src.tar.gz.sha512

Add source packages, signatures and digests to subversion

.. code-block:: bash

  cd $RELEASE_WORKING_DIR
  svn co https://dist.apache.org/repos/dist/dev/incubator/sdap sdap
  mkdir sdap/apache-sdap-1.0.0-rc1
  cp incubator-sdap-nexusproto/apache-sdap-nexusproto-1.0.0-src.tar.gz* sdap/apache-sdap-1.0.0-rc1/.
  cp incubator-sdap-ingester/apache-sdap-ingester-1.0.0-src.tar.gz* sdap/apache-sdap-1.0.0-rc1/.
  cp incubator-sdap-nexus/apache-sdap-nexus-1.0.0-src.tar.gz* sdap/apache-sdap-1.0.0-rc1/.
  cd sdap
  svn add apache-sdap-1.0.0-rc1
  svn ci -m "Uploading release candidate Apache SDAP apache-sdap-1.0.0-rc1 to dev area" apache-sdap-1.0.0-rc1

Start a VOTE thread
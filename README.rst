*******************
Molecule GCE Plugin
*******************

.. image:: https://badge.fury.io/py/molecule-gce.svg
   :target: https://badge.fury.io/py/molecule-gce
   :alt: PyPI Package

.. image:: https://zuul-ci.org/gated.svg
   :target: https://dashboard.zuul.ansible.com/t/ansible/builds?project=ansible-community/molecule-gce

.. image:: https://img.shields.io/badge/code%20style-black-000000.svg
   :target: https://github.com/python/black
   :alt: Python Black Code Style

.. image:: https://img.shields.io/badge/Code%20of%20Conduct-Ansible-silver.svg
   :target: https://docs.ansible.com/ansible/latest/community/code_of_conduct.html
   :alt: Ansible Code of Conduct

.. image:: https://img.shields.io/badge/Mailing%20lists-Ansible-orange.svg
   :target: https://docs.ansible.com/ansible/latest/community/communication.html#mailing-list-information
   :alt: Ansible mailing lists

.. image:: https://img.shields.io/badge/license-MIT-brightgreen.svg
   :target: LICENSE
   :alt: Repository License

Molecule GCE is designed to allow use Google Cloud Engine for
provisioning test resources.

Please note that this driver is currently in its early stage of development.

Installation and Usage
======================

Install molecule-gce and pre-requisites:
   pip install molecule-gce requests google-auth

If you plan on testing Windows instances you also need pywinrm
   pip install pywinrm

Create a new role with molecule using the vmware driver:
   molecule init role <role_name> -d gce

Configure `<role_name>/molecule/default/molecule.yml` with required parameters :
A simple config is:

.. code-block:: yaml
   dependency:
     name: galaxy
   driver:
     name: gce
     project_id: my-google-cloud-platform-project-id #if not set, will default to env GCE_PROJECT_ID
     region: us-central1 #REQUIRED     
     network_name: my-vpc #specify if other than default
     subnetwork_name: my-subnet #specify if other than default
     vpc_host_project: null #if you use a shared vpc, set here the vpc host project. In that case, your GCP user needs the necessary permissions in the host project, see https://cloud.google.com/vpc/docs/shared-vpc#iam_in_shared_vpc
     auth_kind: serviceaccount #set to machineaccount or serviceaccount or application - if set to null will read env GCP_AUTH_KIND
     service_account_email: null #set to an email associated with the project - if set to null, will default to GCP_SERVICE_ACCOUNT_EMAIL. Should not be set if using auth_kind serviceaccount.
     service_account_file: /path/to/gce-sa.json #set to the path to the JSON credentials file - if set to null, will default to env GCP_SERVICE_ACCOUNT_FILE
     scopes: 
       - "https://www.googleapis.com/auth/compute" #will default to env GCP_SCOPES, https://www.googleapis.com/auth/compute is the minimum required scope.
     external_access: false #chose whether to create a public IP for the VM or not - default is private IP only
     instance_os_type: linux #will be considered linux by default, but can be explicitely set to windows. You can not mix Windows and Linux VMs in the same scenario.
   
   platforms:
     - name: ubuntu-instance-created-by-molecule # REQUIRED: this will be your VM name
       zone: us-central1-a # REQUIRED: set it to the zone you want that instance to be in
       machine_type: n1-standard-1 #defines your machine type
       image: 'projects/ubuntu-os-cloud/global/images/family/ubuntu-1604-lts' #Points to an image, you can get a list of available images with command 'gcloud compute images list'. The expected format of this string is 'projects/<project>/global/images/family/<family-name>
' (see https://googlecloudplatform.github.io/compute-image-tools/daisy-automating-image-creation.html)
     - name: debian-instance-created-by-molecule
       zone: us-central1-a
       machine_type: n1-standard-1
       image: 'projects/debian-cloud/global/images/family/debian-10'

   provisioner:
     name: ansible
   verifier:
     name: ansible

.. _get-involved:

Get Involved
============

* Join us in the ``#ansible-molecule`` channel on `Freenode`_.
* Join the discussion in `molecule-users Forum`_.
* Join the community working group by checking the `wiki`_.
* Want to know about releases, subscribe to `ansible-announce list`_.
* For the full list of Ansible email Lists, IRC channels see the
  `communication page`_.

.. _`Freenode`: https://freenode.net
.. _`molecule-users Forum`: https://groups.google.com/forum/#!forum/molecule-users
.. _`wiki`: https://github.com/ansible/community/wiki/Molecule
.. _`ansible-announce list`: https://groups.google.com/group/ansible-announce
.. _`communication page`: https://docs.ansible.com/ansible/latest/community/communication.html

.. _license:

License
=======

The `MIT`_ License.

.. _`MIT`: https://github.com/ansible/molecule/blob/master/LICENSE

The logo is licensed under the `Creative Commons NoDerivatives 4.0 License`_.

If you have some other use in mind, contact us.

.. _`Creative Commons NoDerivatives 4.0 License`: https://creativecommons.org/licenses/by-nd/4.0/

.. role:: red
.. role:: bred

Lab 3.1: Configure an Explicit Proxy deployment through Guided Configuration
----------------------------------------------------------------------------

Minimally an explicit proxy requires DNS settings to be configured. To enable
this in the SSLO UI click :red:`System Settings`.

In the DNS Settings section make the following modifications:

- **DNS Query Resolution** - select :red:`Local Forwarding Nameserver`.

- **Local Forwarding Nameserver(s)** - enter :red:`10.30.0.1`.

- **[Optional] Logging Level** - select the logging level most appropriate for
  the deployment. Keep in mind, however, that DEBUG logging produces an
  enormous amount of local Syslog traffic and is not recommended when
  processing production traffic flows.

- Click :red:`Deploy` to commit the changes.

In the SSL Orchestrator dashboard view, select the :red:`Topologies` tab
(bottom) and click :red:`Add`.

- **Configuration review and prerequisites** - take a moment to review the
  topology options and workflow configuration, then click :red:`Next`.

- **Topology Properties**

  - **Name**: provide some name (ex. ":red:`explicit`")

  - **Protocol**: :red:`TCP`

  - **IP Family**: :red:`IPv4`

  - **Topology**: select :red:`L3 Explicit Proxy`

  - Click :red:`Save & Next`

- **SSL Configurations** - the existing outbound SSL settings from Lab 1 can be
  re-used here.

  - **SSL Profile**: :red:`Use Existing`, select the existing "outbound" SSL
    profile.

  - Click :red:`Save & Next`

    .. warning:: Whenever repurposing a topology setting, a warning will
       appear, "There are other configuration items that are referencing this
       item. Editing this item will affect the referencing ones mentioned
       below".
       
       Click :red:`OK` to acknowledge.

- **Services List** - there are no new services to create.

  - Click :red:`Save & Next`

- **Service Chain List** - there are no new service chains to create.

  - Click :red:`Save & Next`

- **Security Policy** - the existing outbound Security Policy from Lab 1 can be
  re-used here.

  - Select: :red:`Use Existing`, and select the existing "outbound" Security
    policy.
  - Click :red:`Save & Next`
  - Click :red:`OK` to acknowledge the profile warning.

- **Interception Rule** - an explicit proxy requires a unique IP address and
  port listener.

  - **IPV4 Address**: :red:`10.20.0.150`

  - **Port**: :red:`3128`

  - **Access Profile**: if enabling explicit proxy authentication, select an
    existing SWG-Explicit access profile here.

  - **VLANs**: select :red:`client-net`

  - Click :red:`Save & Next`

- **Egress Setting** - traffic egress settings are now defined per-topology and
  manage both the gateway route and outbound SNAT settings.

  - **Manage SNAT Settings** - enables per-topology instance SNAT settings. For
    this lab, select :red:`Auto Map`.

  - **Gateways** - enables per-topology instance gateway routing. Options are
    to use the system default route, to use an existing gateway pool, or to
    create a new gateway. For this lab, select :red:`Use Existing Gateway
    Pool`, then select the ":red:`-ex-pool-4`" gateway pool.

  - Click :red:`Save & Next`

- **Summary** - the summary page presents an expandable list of all of the
  workflow-configured objects. To expand the details for any given setting,
  click the corresponding arrow icon on the far right. To edit any given
  setting, click the corresponding pencil icon. Clicking the pencil icon will
  send the workflow back to the selected settings page.

- When satisfied with the defined settings, click :red:`Deploy`.

- **Testing** - To test the deployed solution, RDP to the
  :bred:`Outbound Win7 Client` and do the following:

  .. hint:: Username = :red:`student` / Password = :red:`agility`

  - Configure the client browser to use :red:`10.20.0.150:3128` for explicit
    proxy access.

  - SSH to BIG-IP CLI and run the following tcpdump command to view the browser
    traffic hitting the explicit proxy IP.

    .. code-block:: bash

       tcpdump -nni client-net port 3128
  
  - Test by browsing to url of choice.  You should see the traffic stream
    across tcpdump from previous command.

  - An explicit proxy request test can also be done using command-line cURL:

    .. code-block:: bash

       curl -vk –proxy 10.20.0.150:3128 https://www.example.com

    Again watch the tcpdump from the previous test to confirm explicit proxy
    use.

External Certificate Authority (CA) Provisioning
================================================

.. _epoexternalcertissuance:

This page describes how to provision a client that uses certificates
signed by an externally managed Certificate Authority (CA) with an ePO-based
DXL fabric.

This is in contrast to certificates that are signed by the internal ePO
Certificate Authority (CA).

    .. note::
    
        While using an external Certificate Authority (CA) is technically possible with an OpenDXL Broker, 
        the actual steps are outside the scope of this documentation.`

The following steps walk through the creation of an external Certificate Authority (CA),
generation of a client key-pair, and export of broker-related information from ePO:

* Create a Certificate Authority (CA) and Client Certificate Files (:doc:`certcreation`)
* Import Certificate Authority (CA) into ePO (:doc:`epocaimport`)
* Export the Broker Certificates (:doc:`epobrokercertsexport`)
* Export the list of DXL Brokers (:doc:`epobrokerlistexport`)

The final step is to populate the contents of a ``dxlclient.config`` file. In this particular case, the
``dxlclient.config`` file that is located in the ``sample`` sub-directory of the Java DXL SDK
will be populated.

The following steps walk through the process of populating this file:

1. Open the ``dxlclient.config`` file located in the ``sample`` sub-directory of the Java DXL SDK.

   The contents should appear as follows:

       .. parsed-literal::

           [General]
           UseWebSockets=false

           [Certs]
           BrokerCertChain=<path-to-cabundle>
           CertFile=<path-to-dxlcert>
           PrivateKey=<path-to-dxlprivatekey>

           [Brokers]
           unique_broker_id_1=broker_id_1;broker_port_1;broker_hostname_1;broker_ip_1
           unique_broker_id_2=broker_id_2;broker_port_2;broker_hostname_2;broker_ip_2

           [BrokersWebSockets]
           unique_websocket_broker_id_1=unique_websocket_broker_id_1;broker_websocket_port_1;broker_hostname_1;broker_ip_1
           unique_websocket_broker_id_2=unique_websocket_broker_id_2;broker_websocket_port_2;broker_hostname_2;broker_ip_2

           #[Proxy]
           #Address=<Proxy host name or IP address>
           #Port=<Proxy port>
           #User=<User name required for authentication with the Proxy>
           #Password=<Password required for authentication with the Proxy>

2. Optionally update the ``UseWebSockets`` value to indicate if the OpenDXL Java Client should connect to DXL Brokers
   via WebSockets. This flag will override the default behavior which is the following:

       MQTT will be used for connections to DXL Brokers if the ``[Brokers]`` section is not empty.

       WebSockets will be used for connections to DXL Brokers if the the ``[Brokers]`` section is empty and the
       ``[BrokersWebSockets]`` section is not empty.


3. Update the ``CertFile`` and ``PrivateKey`` values to point to the certificate file (``client.crt``) and
   private key file (``client.key``) that were created during the certificate provisioning steps.

   See the :doc:`certcreation` section for more information on the creation of client key-pairs.

   After completing this step the contents of the configuration file should look similar to:

       .. parsed-literal::

           [General]
           UseWebSockets=false

           [Certs]
           BrokerCertChain=<path-to-cabundle>
           CertFile=c:\\certificates\\client.crt
           PrivateKey=c:\\certificates\\client.key

           [Brokers]
           unique_broker_id_1=broker_id_1;broker_port_1;broker_hostname_1;broker_ip_1
           unique_broker_id_2=broker_id_2;broker_port_2;broker_hostname_2;broker_ip_2

           [BrokersWebSockets]
           unique_websocket_broker_id_1=unique_websocket_broker_id_1;broker_websocket_port_1;broker_hostname_1;broker_ip_1
           unique_websocket_broker_id_2=unique_websocket_broker_id_2;broker_websocket_port_2;broker_hostname_2;broker_ip_2

           #[Proxy]
           #Address=<Proxy host name or IP address>
           #Port=<Proxy port>
           #User=<User name required for authentication with the Proxy>
           #Password=<Password required for authentication with the Proxy>

4. Update the ``BrokerCertChain`` value to point to the Broker Certificates file (``brokercerts.crt``)
   that was created when exporting the Broker Certificates.

   See the :doc:`epobrokercertsexport` section for more information on exporting Broker Certificates.

   After completing this step the contents of the configuration file should look similar to:

       .. parsed-literal::

           [General]
           UseWebSockets=false

           [Certs]
           BrokerCertChain=c:\\certificates\\brokercerts.crt
           CertFile=c:\\certificates\\client.crt
           PrivateKey=c:\\certificates\\client.key

           [Brokers]
           unique_broker_id_1=broker_id_1;broker_port_1;broker_hostname_1;broker_ip_1
           unique_broker_id_2=broker_id_2;broker_port_2;broker_hostname_2;broker_ip_2

           [BrokersWebSockets]
           unique_websocket_broker_id_1=unique_websocket_broker_id_1;broker_websocket_port_1;broker_hostname_1;broker_ip_1
           unique_websocket_broker_id_2=unique_websocket_broker_id_2;broker_websocket_port_2;broker_hostname_2;broker_ip_2

           #[Proxy]
           #Address=<Proxy host name or IP address>
           #Port=<Proxy port>
           #User=<User name required for authentication with the Proxy>
           #Password=<Password required for authentication with the Proxy>

5. Update the ``[Brokers]`` and ``[BrokersWebSockets]`` sections to include the contents of the broker
   list file (``brokerlist.properties``) that was created when exporting the Broker List.

   See the :doc:`epobrokerlistexport` section for more information on exporting the Broker List.

   After completing this step the contents of the configuration file should look similar to:

       .. parsed-literal::

           [General]
           UseWebSockets=false

           [Certs]
           BrokerCertChain=c:\\certificates\\brokercerts.crt
           CertFile=c:\\certificates\\client.crt
           PrivateKey=c:\\certificates\\client.key

           [Brokers]
           {5d73b77f-8c4b-4ae0-b437-febd12facfd4}={5d73b77f-8c4b-4ae0-b437-febd12facfd4};8883;mybroker.mcafee.com;192.168.1.12
           {24397e4d-645f-4f2f-974f-f98c55bdddf7}={24397e4d-645f-4f2f-974f-f98c55bdddf7};8883;mybroker2.mcafee.com;192.168.1.13

           [BrokersWebSockets]
           {5d73b77f-8c4b-4ae0-b437-febd12facfd4}={5d73b77f-8c4b-4ae0-b437-febd12facfd4};443;mybroker.mcafee.com;192.168.1.12
           {24397e4d-645f-4f2f-974f-f98c55bdddf7}={24397e4d-645f-4f2f-974f-f98c55bdddf7};443;mybroker2.mcafee.com;192.168.1.13

           #[Proxy]
           #Address=<Proxy host name or IP address>
           #Port=<Proxy port>
           #User=<User name required for authentication with the Proxy>
           #Password=<Password required for authentication with the Proxy>

6. Optionally update the ``[Proxy]`` section to have the host name or IP address, port, user name, and
   password of the proxy that connections to DXL Brokers will be routed through. The ``User`` and ``Password``
   values are not required if the proxy does not require authentication.

   After completing this step the contents of the configuration file should look similar to:

       .. parsed-literal::

          [General]
          UseWebSockets=false

          [Certs]
          BrokerCertChain=c:\\certificates\\brokercerts.crt
          CertFile=c:\\certificates\\client.crt
          PrivateKey=c:\\certificates\\client.key

          [Brokers]
          {5d73b77f-8c4b-4ae0-b437-febd12facfd4}={5d73b77f-8c4b-4ae0-b437-febd12facfd4};8883;mybroker.mcafee.com;192.168.1.12
          {24397e4d-645f-4f2f-974f-f98c55bdddf7}={24397e4d-645f-4f2f-974f-f98c55bdddf7};8883;mybroker2.mcafee.com;192.168.1.13

          [BrokersWebSockets]
          {5d73b77f-8c4b-4ae0-b437-febd12facfd4}={5d73b77f-8c4b-4ae0-b437-febd12facfd4};443;mybroker.mcafee.com;192.168.1.12
          {24397e4d-645f-4f2f-974f-f98c55bdddf7}={24397e4d-645f-4f2f-974f-f98c55bdddf7};443;mybroker2.mcafee.com;192.168.1.13

          [Proxy]
          Address=proxy.mycompany.com
          Port=3128
          User=proxyUser
          Password=proxyPassword

7. At this point you can run the samples included with the Java SDK.
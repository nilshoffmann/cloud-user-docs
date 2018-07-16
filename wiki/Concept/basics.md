## Cloud concepts: Domain, User, Project, Role, Image, Flavor, Network, Instance, Volume, Key pairs

Before using the cloud, a user needs to be familar with certain concepts. Most of these concepts are applicable to almost all clouds (openstack based private clouds, Amazon EC2, Microsoft Azure....):

##### Domain

The highest management layer in the latest OpenStack releases are ''domains''. A domain is a collection of users, project, roles, role assignment, common configurations like quotas etc. The domain concept allow cloud providers to grant administrative privileges to certain users, and thus enable them to manage their "own" cloud instance.

In the case of de.NBI, every cloud user needs an Elixir account and must be registered at the de.NBI cloud portal. 

The domain concept also allows the creation of additional domains, e.g. for running a workshop with dedicated users . These domains can be created on request.

##### User

A *user* is an entity within a domain. A user is able to authenticate itself with the domain name, its user name and a password. Access to the BCF cloud depends on a valid user account. Certain resources in OpenStack are managed on user based, e.g. access keys, virtual machine instances etc.

##### Project

A *project* is a collection of resources like users, images, volumes etc. It is initially created by the domain or cloud administrator, and access to the project is granted to individual users. Resources are associated to a project and are subject to quotas on project level (e.g. number of instance or volume space).

##### Role

A role authorizes a user to access a project and grants certain right to the user within the project scope. The standard roles in OpenStack are ''admin'' and ''member''; with the later one granting standard access and the first one giving administrative privileges within a project.

A user needs to be a member of a project (needs to have a role associated) to access the OpenStack web interface.

##### Image

An *image* is the binary representation of a computer's hard disk and defines the content of a virtual machine. Images may be available in various different formats (raw, qcow2, vmdk, ...) and are used to populate the first hard disk of virtual machines. Ready-to-use images for various purposes are available for download in the internet, or can be created using a wide variety of tools.

Images may be associated to a project, or made publically available for all projects. Upon uploading an image the minimal requirements like hard disk size or available memory can be specified to ensure that the image is used with the right amount of resources.

More information about images are in available in the [Image overview](images.md).

##### Flavor

A *flavor* defines a virtual machine setup by defining parameters like hard disk size, available memory or CPU core number. The cloud provider use flavors to optimize virtual machines for the available hardware. A flavor may also be used to restrict virtual machines to certain physical hosts, e.g. by requiring extra local storage or feature like a GPU.

The following flavors are provided in *all* de.NBI cloud sites under the same name with the same resources. Flavors with ephemeral storage can *optionally* be provided if supported by the cloud center.
The flavor name is the name of the "regular" de.NBI flavor with the postfix " + ephemeral", e.g. "de.NBI medium + ephemeral".  

Name | # VCPUs | RAM \[GB] | Root disk \[GB] | Ephemeral disk \[GB] |
---|---:|---:|---:|---:|
de.NBI default | 2 | 4 | 20 | N/A
de.NBI small | 8 | 16 | 20 | 50 |
de.NBI medium | 16 | 32 | 20 | 150 |
de.NBI large | 32 | 64 | 20 | 250 |

Depending on the cloud site additional flavors are available according to the allocatable hardware (High Memory, GPU, FPGA, ...). 

Other flavors will be provided as needed or requested.

##### Network

While a flavor defines the parameters of a single virtual machine, a network defines how virtual machines are connected to each other or to the internet. Each virtual machine needs at least one network connection. More information about networks are in available in the [network overview](network.md).

##### Instance

An instance is running virtual machine defined by an image (what to run), a flavor (which amount of resources) and a network (how to connect to it). 

##### Volume

Hard disks of virtual machines are not persistent. After the virtual machine is terminated, its hard disk's content is deleted and not available anymore. A volume in contrast provides a persistent way to store data. It is created separately from a virtual machine and attached to an instance at runtime. It may also be detached and attached to another instance at will. The volume acts as another block device from the point of view of the virtual machine, so users are not restricted to certain formats.

In the current configuration, the same volume cannot be attached to multiple virtual machines (in the same way a physical hard disk can only be connected to one compute). To share the content of volume, snapshots can be made and different snapshots can be attached to different instances.

Volume are part of a project and cannot be shared; a transfer of volumes or snapshots between projects it possible.

##### Key pair

A key pair are credentials defined by a user to access a virtual machine. It usually consists of a SSH key pair, with the public key being configured within an instance during startup, and the private key being accessible to the user only. The user can use the key pair to access an instance via SSH over the network.

Without a key pair, a user can only access a virtual machine's console by using the console redirection feature of the OpenStack dashboard. Many available images for virtual machines require the user to define a SSH key pair since no standard user account is configured.

If in doubt, define and use a keypair.

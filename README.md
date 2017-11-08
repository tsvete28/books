# books
This is just a playground project.
## Infrastructure Micro Template Reference

You can find sample infrastructure micro templates at https://github.com/SoftwareAG/sagdevops-templates
```
alias: sag-spm               #REQUIRED A unique alias for the template.
                             #Validated, do not use ^\w+[\w\-\.]*$
description:                 #Optional description of the template 

environments:
  default:  
    nodes: ${}               #REQUIRED For local bootstrap: The unique alias of a new node. Example: dev${spm.port}
                             #For remote bootstrap: a list of the hostnames of the remote hosts. Example: host1,host2
    install.dir: ${}         #REQUIRED For local bootstrap: A unique installation directory.Example: /home/user/dev${spm.port}
                             #For remote bootstrap: the installation directory on the remote hosts. Example: /home/user/softwareag
    cc.installer: ${}        #REQUIRED The Command Central bootstrap installer to use for bootstrapping Platform Manager.
    spm.port: 8092           #The HTTP port of Platform Manager. Default: 8092
    os.credentials.key: ${}  #For remote bootstrap: The SSH credentials to use to connect to the remote hosts.
    spm.credentials.key:     #The credentials alias for the Platform Manager "Administrator" user. Default: DEFAULT_ADMINISTRATOR

layers:
  infra:                     #REQUIRED The definition of the infrastructure layer
    templates:
      - spm                  #The alias of the Plaform Manager inline template defined in "templates".

templates:
  spm:                       #An alias for the Platform Manager inline template. Validated, do not use ^\w+[\w\-\.]*$
    products:
      SPM:                   #The product ID of Platform Manager
        OSGI-SPM:            #REQUIRED Registers OSGI-SPM as an infrastructure layer instance.

provision:
  default:
    infra: ${nodes}          #A node (for local bootstrap) or a list of nodes (for remote bootstrap) on which to provision the infrastructure layer.

nodes:                       #Details about how to install Platform Manager.
  default:
    ${node}:
      host: localhost        #For local bootstrap: The same host as the host on which Command Central runs.
      port: ${spm.port}      #The number of the Platform Manager port.
      secure: false                   # Whether to use HTTPS (true) or HTTP (false). Always start with HTTP.
      credentials: ${spm.credentials.key}  #The credentials alias for the Platform Manager "Administrator" user.
      bootstrapInfo:
        installer: ${cc.installer}    #The Command Central bootstrap installer to use for bootstrapping Platform Manager.
        installDir: ${install.dir}    #The installation directory of Platform Manager.
        credentials: ${os.credentials.key} #For remote bootstrap: the SSH credentials to connect to the remote hosts.
        version: "${release}"        # The release version of the Platform Manager node. For backward compatibity when "cc.installer" is not available.
```

# Salt Calls Cheatsheet

**Gitlab Changes Detection Project**

```
**SALT HIGHSTATE CALL**

**USEFUL SALT DEBUG CALLS**
salt-call state.sls aerospike.nb6.as1xx.config -l debug test=y
salt-call state.sls aerospike.nb6.as1x.config -l debug test=y
salt-call state.sls aerospike.nb6.aerospike-salt.config -l debug test=y
salt-call state.sls aerospike.prometheus -l debug test=y
salt-call state.sls aerospike.grafana.install -l debug test=y
```

## FILES TO BE CHECKED

```
- aerospike.conf
- astools.conf
```

### Prometheus Salt

```
salt-call state.sls aerospike.grafana.install -l debug test=y
```

## 1- MAIN AEROSPIKE FILE - aerospike.conf

```
security {
    enable-security true
}

service {

        cluster-name aerospike-salt
        paxos-single-replica-limit 1 # Number of nodes where the replica count is automatically reduced to 1.
        log-local-time true
        proto-fd-max 15000
        feature-key-file /etc/aerospike/features.conf

}

logging {
    file /var/log/aerospike/aerospike.log {
        context any info
        context migrate debug
    }
}

network {

        tls phonepeaerospike {
		cert-file /etc/aerospike/ssl/server.crt
		key-file /etc/aerospike/ssl/server.key
		ca-path /etc/aerospike/ssl/ca.crt
	}

        service {

                tls-address 10.57.14.57
                tls-port 4333
                tls-authenticate-client false
                tls-name phonepeaerospike

        }

        heartbeat {

                mode mesh
                address 10.57.14.57
                port 3002

                mesh-seed-address-port 10.57.14.57 3002
                
                interval 150
                timeout 10
        }

        fabric {
                tls-address 10.57.14.57
                tls-port 3011
                tls-name phonepeaerospike
        }

        info {
                port 3003
        }
}

namespace test {

        replication-factor 2
        memory-size 1G
        default-ttl 1d # 30 days, use 0 to never expire/evict.
        storage-engine memory

}
```

## AEROSPIKE - SALT CALL FILE

```
local:
----------
          ID: /var/local/aero_config.pl
    Function: file.managed
      Result: None
     Comment: The file /var/local/aero_config.pl is set to be changed
              Note: No changes made, actual changes may
              be different due to other states.
     Started: 22:52:31.217450
    Duration: 367.421 ms
     Changes:   
              ----------
              diff:
                  --- 
                  +++ 
                  @@ -3,15 +3,27 @@
                   use strict; 
                   use YAML::Syck; 
                   use Data::Dumper ; 
                  +use Filesys::DiskSpace;
                   
                   my $aero_config = '/var/local/aero_config' ;
                   my $host_config = '/var/local/aero_config.yml' ;
                   my $output_file = '/etc/aerospike/aerospike.conf'; 
                   
                  -my $hostname = '{{ salt['grains.get']('nodename') }}.{{ salt['grains.get']('domain') }}' ; 
                  +my $hostname = 'stg-aerospikehrohit001.phonepe.nb6' ; 
                   my $aero_config_R = '/var/local/aerospike_riemann_config' ;
                   my $output_file_R = '/var/local/aerospike.sh' ;
                   chomp $hostname; 
                  +
                  +
                  +# file system /home or /dev/sda5
                  +my $dir = "/var/lib/aerospike";
                  +
                  +# get data for /home fs
                  +my ($fs_type, $fs_desc, $used, $avail, $fused, $favail) = df $dir;
                  +
                  +print $avail ;
                  +
                  +
                   
                   
                   open F,$aero_config ; 
                  @@ -53,14 +65,15 @@
                   	}
                   	else
                    	{
                  -	        $cluster_r .= "$ip:3000 , "; 
                  +	        $cluster_r .= "$ip:3000,"; 
                   	}
                   
                          ++$i ; 
                   }
                   
                   if(!defined $this_host) {
                  -	die "Congif error: $hostname  not part of cluster\n"; 
                  +	die "Congif error: $hostname  not part of cluster\n";
                  +
                   }
                   
                   $aero_conf_l =~ s/__LOCALHOST__/${this_host}/gs ; 
                  @@ -79,7 +92,7 @@
                   
                   sub get_ip {
                   	my $h = shift; 
                  -	my $cmd = `host $h` ; 
                  +	my $cmd = `host $h | grep address` ;
                   	my @Lines = split/\n/,$cmd ; 
                   	my $ip = (split/\s+/,$Lines[-1])[-1];
                           print $ip ;
----------
          ID: /var/local/aero_config.yml
    Function: file.managed
      Result: None
     Comment: The file /var/local/aero_config.yml is set to be changed
              Note: No changes made, actual changes may
              be different due to other states.
     Started: 22:52:31.585308
    Duration: 14.648 ms
     Changes:   
              ----------
              diff:
                  --- 
                  +++ 
                  @@ -1,4 +1,4 @@
                   ---
                  -- prd-aerospike001
                  -- prd-aerospike002
                  -- prd-aerospike003
                  +- stg-projectrohit001.phonepe.nb6
                  +- stg-projectrohit002.phonepe.nb6
                  +- stg-projectrohit003.phonepe.nb6
----------
          ID: /var/local/aero_config
    Function: file.managed
      Result: None
     Comment: The file /var/local/aero_config is set to be changed
              Note: No changes made, actual changes may
              be different due to other states.
     Started: 22:52:31.600331
    Duration: 2149.621 ms
     Changes:
              ----------
              diff:
                  --- 
                  +++ 
                  @@ -1,51 +1,72 @@
                  +security {
                  +    enable-security true
                  +}
                  +
                  +service {
                  +        cluster-name aerospike101
                  +        paxos-single-replica-limit 1 # Number of nodes where the replica count is automatically reduced to 1.
                  +        proto-fd-max 15000
                  +        log-local-time true
                  +        
                  +        node-id ccece607e322525f
                  +        feature-key-file /etc/aerospike/features.conf
                  +}
                  +
                  +
                   network {
                  -    	service {
                  -            	address any
                  -            	port 3000
                  -            	access-address __LOCALHOST__
                  -            	network-interface-name eth0
                  -		migrate-threads 4
                  -                paxos-recovery-policy auto-reset-master
                  -                keepalive-time  600
                  -                service-threads 16
                  +	tls phonepeaerospike {
                  +		cert-file /etc/aerospike/ssl/server.crt
                  +		key-file /etc/aerospike/ssl/server.key
                  +		ca-path /etc/aerospike/ssl/
                  +	}
                   
                  -    	}
                  +        service {
                  +                tls-port 4333
                  +                tls-address 10.57.14.57
                  +                tls-authenticate-client false
                  +                tls-name phonepeaerospike
                  +        }
                   
                       	heartbeat {
                               	mode mesh
                               	port 3002
                   		__CLUSTER__
                  -            	interval 3000
                  +		address 10.57.14.57
                  +
                  +            	interval 150
                               	timeout 10
                       	}
                  -       fabric {
                  -           port 3001
                  +        fabric {
                  +		tls-address 10.57.14.57
                  +                tls-port 3011
                  +                tls-name phonepeaerospike
                           }
                           info {
                              port 3003
                           }
                   }
                  -namespace revolver {
                  -     replication-factor     2
                  -     memory-size            512M
                  -     default-ttl            0
                  -     max-ttl                0
                  -     read-consistency-level-override one
                  -     write-commit-level-override all
                  -     high-water-disk-pct    90
                  -     high-water-memory-pct  90
                  -     storage-engine device {
                  -       file /mnt/aerospike/pp.dat
                  -       filesize 5G
                  -       data-in-memory true
                  -       write-block-size 1M
                  -       scheduler-mode noop
                  +
                  +logging {
                  +	file /var/log/aerospike/aerospike.log {
                  +		context any info
                  +		context migrate debug
                  +	}
                   }
                   
                  -logging {
                  -    file /var/log/aerospike/aerospike.log {
                  -        context any info
                  -        context migrate debug
                  -    }
                  - }
                  -
                  +namespace test {
                  +        memory-size 1G
                  +        replication-factor 2
                  +        default-ttl 0
                  +        high-water-disk-pct 70
                  +        high-water-memory-pct 70
                  +        stop-writes-pct 90
                  +        nsup-period 120
                  +        storage-engine device {
                  +                file /var/lib/aerospike/test.dat
                  +                filesize 2G
                  +                data-in-memory true
                  +                write-block-size 128K
                  +                defrag-lwm-pct 50
                  +                defrag-startup-minimum 10
                  +        }
                  +}
----------
          ID: /etc/aerospike/features.conf
    Function: file.managed
      Result: None
     Comment: The file /etc/aerospike/features.conf is set to be changed
              Note: No changes made, actual changes may
              be different due to other states.
     Started: 22:52:33.750350
    Duration: 14.376 ms
     Changes:   
              ----------
              diff:
                  --- 
                  +++ 
                  @@ -1,3 +1,5 @@
                  +# generated 2020-02-03 14:41:48
                  +
                   feature-key-version              1
                   serial-number                    126665143
                   
                  @@ -8,3 +10,4 @@
                   MEYCIQCXME3IqKO3lF19JBTFGoLgAomxHlydRf8W6IswHCzaPQIhALbJHyUbZqjE
                   XIrE6+u7DWH9S0/3gc3sGaQtDBDA9eBvJA==
                   ----- END OF SIGNATURE -----------------------------------------
                  +
              mode:
                  0755
----------
          ID: run-aerospike-config
    Function: cmd.run
        Name: /var/local/aero_config.pl
      Result: None
     Comment: Command "/var/local/aero_config.pl" would have been executed
     Started: 22:52:33.766207
    Duration: 1.755 ms
     Changes:   
----------
          ID: /etc/logrotate.d/aerospike
    Function: file.managed
      Result: None
     Comment: The file /etc/logrotate.d/aerospike is set to be changed
              Note: No changes made, actual changes may
              be different due to other states.
     Started: 22:52:33.768321
    Duration: 5.464 ms
     Changes:   
              ----------
              newfile:
                  /etc/logrotate.d/aerospike
----------
          ID: /etc/aerospike/astools.conf
    Function: file.managed
      Result: None
     Comment: The file /etc/aerospike/astools.conf is set to be changed
              Note: No changes made, actual changes may
              be different due to other states.
     Started: 22:52:33.774197
    Duration: 5.472 ms
     Changes:   
              ----------
              mode:
                  0755

Summary for local
------------
Succeeded: 7 (unchanged=7, changed=6)
Failed:    0
------------
Total states run:     7
Total run time:   2.559 s
```
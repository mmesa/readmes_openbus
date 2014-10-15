#LOGSTASH - CONFIGURACION ACTUAL




```
input {
  syslog{
    type => syslog
    port => 514
  }
}

filter {
  grok {
    match => { "message" => "((\S+)\|\|)?(?<content>(.*))"}
  }
  mutate {
    replace => ["message", "%{content}"]
  }
}

output {
        if [program] == "Multientidad.NetIX.LogicalEntity.ColaborativeServer.production.postfix.postfix"{
                kafka{
					topic_id => "ob_src_postfix"
					broker_list => "180.133.240.165:9092,180.133.240.166:9092,180.133.240.167:9092"
					codec => plain {
						format => "%{message}"
					}
                }
    }
    else if [program] == "Multientidad.NetIX.LogicalEntity.ColaborativeServer.production.postfix.amavis"{
                kafka{
					topic_id => "ob_src_amavis"
					broker_list => "180.133.240.165:9092,180.133.240.166:9092,180.133.240.167:9092"
					codec => plain {
						format => "%{message}"
					}			
                }
    }
    else if [program] == "Multientidad.NetIX.CommunicationSystems.MTA.production.IronPort.mail"{
                kafka{
					broker_list => "180.133.240.165:9092,180.133.240.166:9092,180.133.240.167:9092"
					topic_id => "op_src_ironport"
					codec => plain {
						format => "%{message}"
					}
                }
    }
    else if [program] == "Multientidad.NetIX.CommunicationSystems.Proxy.production.Bluecoat.bopxnxusrsp01.main"{
                kafka{
					topic_id => "ob_src_bluecoat"
					broker_list => "180.133.240.165:9092,180.133.240.166:9092,180.133.240.167:9092"
					codec => plain {
						format => "%{message}"
					}
                }
	}
    else if [program] == "Multientidad.NetIX.CommunicationSystems.Proxy.production.Bluecoat.bepxnxusrsp01.main"{
                kafka{
                                        topic_id => "ob_src_bluecoat"
                                        broker_list => "180.133.240.165:9092,180.133.240.166:9092,180.133.240.167:9092"
                                        codec => plain {
                                                format => "%{message}"
                                        }
                }
        }

	else{
		file{
			path => '/root/software/logstash-1.4.1/error.out'
		}
	}
}

```

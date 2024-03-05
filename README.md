# Elastic Post Processor
----
## About This Pack


This pack is designed to help map event fields into the OCSF data format (https://schema.ocsf.io/?extensions=) as a post-processor for specific destinations (verify that your downstream destination supports this data schema before sending events). Please keep in mind this is primarily a development resource and is only partially functional. Its goal is to provide tooling and references for how to perform the needed mapping for OCSF.


This pack has two major elements to it:
1. Mapping of events to Elastic datastreams
2. Mapping of events to Elastic friendly dataformats
 
Currently supported datasets for mapping to Elastic datastreams
* AWS (from the list below)
* PAN Next-Gen Firewall and Cortex
* Trellix EPO Cloud and EDR Cloud
* ZScaler ZIA and ZPA


Currently, the only supported ***AWS*** datasets for mapping to Elastic datastreams are:
* Cloudfront
* Cloudtrail
* CloudWatch
* EC2 (via Cloudwatch)
* ELB
* Network Firewall
* Route 53 public DNS queries
* Route 53 resolver queries
* S3 server access
* VPC Flow Logs
* WAF


Further details on this mapping is available on [Elastic’s docs for kinesis](https://www.elastic.co/guide/en/kinesis/current/aws-firehose-setup-guide.html#aws-firehose-config-destination-settings)


## Deploying this Pack into Production
Most packs are standalone packs that are designed to work regardless of destination, this pack is different. It is a post processing pack that is tied to specific destinations, potentially requiring the use of other packs in conjunction with this one. Setup is broken up into three stages.


### 1. Environment Setup


Before you begin, ensure that you have met the following requirements:
* Elastic integrations installed for all datasets you want to send via Cribl
* A route for all data to be sent to Elastic using original events (reformatting of events is not supported with this pack)
* ***Recommended:*** Cribl integration for Elastic is installed




### 2. Update Route Filters


The current route filter for the **Map_events_to_Datastreams** pipeline is generic to all AWS inputs, this will need to be adjusted to match the specific data sources you want sent to this pipeline. For the best results we recommend tying your filters to the inputId, this should look like: `__inputId.startsWith('aws:')`


### 3. Updating Pipeline Filters


If you are planning to use this pack to send AWS data to Elastic datastreams, then you will need to review and update the filters used on each eval function for assigning datastreams to events in the **Map_events_to_Datastreams** pipeline. 


It's best to use your own sample events to do this mapping. For the best results we recommend tying your filters to the inputId, this should look like: `__inputId.startsWith('aws:')`


### 4. ***(Optional)*** Add More Datastream Mappings 


If you have more sources you want mapped into Elastic datastreams, then  you will need to do the following:
1. Identify the dataset you want to map into a datastream
2. Clone a ***XXX_events_to_datastreams*** pipeline.
3. Replace the evals events with evals for each event-to-datastream pairing (repeat for each data source you want mapped into datastreams).
4. Add each pipeline to the pack routes with filters for each source.
5. Share your pipeline with the community!


### 3. Attaching to a Destination
To this point, you have reviewed and prepared the Pack for use. In this step, we enable it by tying it to one or more Syslog Sources.


To set the Pack's Pipeline to use your Sources:


1. In the Worker Group's **Manage Destinations** page, select a Elastic (or ECS supporting) Destination where you desire prost-processing - probably, most of them.
2. In the **Post-Processing** tab, select the `[Pack] cribl-elastic-output (Elastic Post Processor)` Pipeline.
3. Repeat steps 1 and 2 for additional Elastic Destinations as needed.
4. Click **Commit & Deploy** when testing is complete.




## Upgrading this Pack


Upgrading certain Cribl Packs using the same Pack ID can have unintended consequences. See [Upgrading an Existing Pack](https://docs.cribl.io/stream/packs#upgrading) for details.


Because this Pack has user-modified items (Pipelines, Functions, and Lookups), Cribl recommends that you install future versions with a unique Pack ID by appending the version number to the Pack ID during import. This allows side-by-side comparisons as you configure the updated version.


Once the update is imported, configure the newly installed Pack:
1. Review the Pipeline's Functions, enabling and disabling functions as needed.
2. Move over any pipelines or functions you may have created (making sure to test they work with the pack updates).
3. Update your Elastic Destinations **Post-Processing** tab’s setting to use the newer pack version.
4. Click **Commit & Deploy** when testing is complete.


## Release Notes
___


### Version 1.0.0 - 2024-02-23
Initial release of the Elastic Post-Processor pack, awesome features include things like:
* AWS logs to Elastic datastreams pipeline
* PAN NGFW and Cortex logs to Elastic datastreams pipeline
* Trellix EPO and EDR logs to Elastic datastreams pipeline
* ZSclaer ZIA and ZPA logs to Elastic datastreams pipeline
* Trellix on-prem XML events to JSON pipeline
* General data formatter for Elastic


___
## Contributing to the Pack
Send a message on our community Slack channel [#packs](https://cribl-community.slack.com/archives/C021UP7ETM3)


## Contact
To contact me, my Cribl community slack handle is: [Alex Cain (cribl.io)](https://cribl-community.slack.com/team/U01C35EMQ01)


Or feel free to send me an email: <acain@cribl.io>


## License
This Pack uses the following license: [`Apache 2.0`](https://github.com/criblio/appscope/blob/master/LICENSE).
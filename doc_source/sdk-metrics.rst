.. Copyright 2010-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.

   This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0
   International License (the "License"). You may not use this file except in compliance with the
   License. A copy of the License is located at http://creativecommons.org/licenses/by-nc-sa/4.0/.

   This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
   either express or implied. See the License for the specific language governing permissions and
   limitations under the License.

.. _sdk_metrics:

|CSM| in the |sdk|
==================

.. meta::
   :description: Configure an agent for |CSM| for Enterprise Support with the |sdk|.
   :keywords: |sdk|, |CSM| for Enterprise Support with |language|, use |language| to monitor AWS Services

|CSMmerge| enables enterprise customers to collect metrics from AWS SDKs on their hosts
and clients shared with AWS Enterprise Support.
|CSM| provides information that helps speed up detection and diagnosis of issues occurring in connections
to AWS services for AWS Enterprise Support customers.

As telemetry is collected on each host, it is relayed via UDP to localhost,
where the |CW| agent aggregates the data and sends it to the |CSM| service.
Therefore, to receive metrics, you must add the |CW| agent to your instance.

.. Learn more about |CSM| in the |CSM_CW|.

The following topics describe how to authorize, set up and configure, and define |CSM| in the |sdk|.

.. toctree::
   :titlesonly:
   :maxdepth: 1

   Authorize SDK Metrics <authorize-metrics>
   Set Up SDK Metrics <setup-metrics>
   SDK Metric Definitions <define-metrics>

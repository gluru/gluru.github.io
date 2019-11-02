# How to implement a data fetcher

This document explains the basic principles of how to implement a data fetcher
plugin (DF) to inject data into Kare MIND. Data fetchers are plugins which keep
MIND in synch with external data sources.
This document assumes that a third party data source has data or resources
which can be imported as MIND documents.

# Authentication

DFs need to authenticate both with the third party source and with MIND in
order to read data from one and import it in the other.

The DF needs to authenticate with the third party platform as required by the
vendor. This authentication depends on the platform and it is outside our
control. If the DF runs as a plugin inside the third party platform the
authentication credentials might already be passed on.

The DF needs to authenticate with MIND APIs.
Please refer to the [documentation](http://gluru-docs.s3-website-eu-west-1.amazonaws.com/public/)
for details on how to use the MIND APIs.

# Importing documents

## First import

Upon a successful authentication in both platform the DF needs to synchronise
them by reading all existing resources in one and pushing it to the other.
This process is as simple as reading all resources and posting them
sequentially to MIND.

We advise to import sequentially or with a low parallelism. The APIs will
throttle aggressive clients. Also the data source will rate limit the requests.

While the data is being imported file changes might occur. This is normal and
the DF should send updates at the end of the first import.

## Keeping documents up to date

Keeping documents in sync is the main purpose of the DF. Generally speaking
there are two ways: processing update events or updating in batch. This very
much depends on the APIs of the third party platform.

## Using third party update events

The best way to keep documents in sync is to use update events. Not all
platforms offer this feature but if they do, this is the most efficient way
to implement it.

Note when using update events to keep documents in sync, events need to be
collected before the first import in order to avoid missing those events which
have occurred between the beginning and the end of the import. This can be
easily done by subscribing to events before the first import but only starting
to consume them at the end of the import.

Note that since the MIND APIs are idempotent applying the same event more than
once won’t change the state of a document as long as events are applied in the
correct order.

## Updating in batch

For those platforms which do not support update events the implementation is a
little bit more computationally intensive.

The most brutal implementation is:
 1. Fetch all the existing documents from the third party source
 2. Fetch all existing documents
 3. Put all the existing ones
 4.  Delete all the ones which no longer exist in the third party source.

Alternatively the DF could try to sort documents by the last change and record
the timestamp of the last update.

The update frequency depends on the quantity of data and how frequently it
changes. We advise updating it at least once every 6 hours.

# UI/UX

DF can be build either standalone or as plugin of existing marketplace
(i.e. for instance a Salesforce plugin).

If built standalone the UI should:
 *  Allow the user to authenticate with the third party including all additional credentials
 *  Allow the user to authenticate with MIND
 *  Allow the user to select the MIND region unless hardcoded or preselected.

If built as a plugin of an existing ecosystem the user will already be
authenticated with the third party and therefore only steps #2 and #3 are required.

# Additional considerations

Some final details.

## Hosting


DF is an extremely data intensive component which should ideally be hosted in
the same geographical area as either the third party service or MIND. It should
also be deployed in a data centre capable of transferring that amount of data.
Ideally the DF should be deployed in different geographical areas and hit the
MIND environment in which the customer data is stored.

## Data privacy considerations

The DF only needs to store the credentials that are used to transfer data, and
doesn’t need to persist any other data.

## White labelling the product

MIND Widget and Admin Console are branded products while the APIs are not
branded. Integrators willing to hide Kare’s brand from their customers should
use the APIs.

# Magento - case study

Magento has a marketplace which contains customer supports plug-ins,
including FAQs modules and live chat. Most of those plugins are commercial.

A MIND-Magento plugin can be built to:
Load the widget inside Magento by adding the widget javascript into the page
template. This would enable the widget to appear and be used in some or in all
pages of Magento.
Export product and pages content from Magento to MIND using the documents APIs.
This would allow Magento content to be searchable by MIND.
Import MIND documents indexed from other sources (Oracle, Salesforce, Zendesk)
or MIND responses as content in Magento. This would allow content from other
sources connected to MIND to be presented as Magento content, for instance to
create a knowledge base or an FAQ page.
Use MIND resolutions to apply custom actions leveraging Magento APIs.
This would allow to connect MIND responses with custom actions implemented by
this plugin.

## Plugin structure

A possible implementation of a magento plugin will have two components a
frontend living inside Magento and built as a Magento extension and a web
service to do the knowledge fetching.

## How to apply custom actions

Custom actions can be achieved by following these steps. When a new account is
created or connected to this plugin, using the /v2/resolutions for each action
to be supported a resolution should be created with, in the resolution body a
permalink to the effect of an action which the user will be prompted to perform.

For instance a resolution “What’s my balance?” can be resolved by prompting the
URL in which the user will find its balance.

Useful links:
 *  https://marketplace.magento.com/extensions/customer-support.html
 *  https://devdocs.magento.com/videos/fundamentals/create-a-new-module/
 *  https://www.magestore.com/magento-2-tutorial/magento-extension-development/

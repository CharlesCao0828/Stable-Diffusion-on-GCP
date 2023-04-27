# Stable Diffusion Model Fine-tuning on Vertex AI Pipeline Quick Start

This guide gives a simple example of how to orchestrate an end to end ML workflow by using Vertex AI pipeline.

As the pictures shows, in this example we can use a one-stage Vertex AI pipeline to train a stable diffusion Lora model on the Kohya environment, and sync the Lora model files automatically to GCS or Cloud Filestore. In this one-stage method, we need to manually push a Kohya Docker image to the specified Docker registry.

If the Kohya docker image hasn’t been uploaded before, we can also chose to use two-stage Vertex AI pipeline, this first stage in Pipeline will build a Kohya Docker image and automatically push to the google managed Docker registry(Artifact Registry), and then the second stage will start a Vertex AI training job, the training job will also automatically sync  the trained Lora file to GCS or Cloud Filestore.

Whether to choose the one-stage or two-stage pipeline can be configured by setting the docker_build parameter in pipeline_conf.yaml.

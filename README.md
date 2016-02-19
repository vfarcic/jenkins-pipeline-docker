This article is the follow-up from the [Jenkins Pipeline](TODO) post.

Now that we understand [the need for the Jenkins pipeline](TODO) and we got familiar with the principles behind the [Jenkins Pipeline Plugin](TODO), we should proceed with some hands-on experience. After all, theory without practice is useful. Please note that we'll explore the pipeline together with the [CloudBees Docker Pipeline Plugin](CloudBees Docker Pipeline Plugin) and that I will assume that you are familiar with the Docker ecosystem. If you aren't, this blog has a lot of resources that will get you up and running with Docker. Do not worry if you are not very proficient with Docker. The purpose of this article is to teach you *Jenkins Pipeline* basics, and we won't go into complex Docker scenarios. I will also assume that you already have [Git](https://git-scm.com/) and [Vagrant](https://www.vagrantup.com/) installed. We'll use the later to create the VM that we'll use as the fully provisioned environment.

Since I prefer hands-on learning, we'll combine *Jenkins Pipeline* with *Docker* to produce an effective and easy to maintain solution for the full continuous deployment lifecycle. We'll see how we can utilise the combination of the Jenkins Pipeline with Docker to run all kinds of tests. We'll run front-end unit tests against Firefox and back-end functional tests that depends on MongoDB. We'll call them pre-deployment tests. Once our pre-deployment tests are successful, we'll run the process that will build a JAR artefact and prepare front-end static files for packaging. With the artefacts ready, we'll proceed, and build the container image that will host our service. Once built, we'll push it to the private registry so that it is available for anyone with the access to our server. Next, we'll add a process that will require a manual confirmation before the container is deployed to production. Once we receive the confirmation, we'll switch to the production server and pull the latest release from the registry. With the latest released pulled, we'll proceed and run the service container as well as those it depends on. Finally, we'll run another round of tests to confirm that the process was indeed executed correctly. We'll call them post-deployment or integration tests. How does that sound for a project? Shall we give it a try?

We'll start by cloning the code and creating virtual machines

```bash
git clone https://github.com/vfarcic/jenkins-pipeline.git

vagrant up cd
```

Now that we have the environment provisioned with Jenkins, slaves, Docker, and Docker Compose, we can start working on our first pipeline. Creating a new Pipeline job is easy and follows the same process like any other Jenkins job.

Creating A Pipeline Job
=======================

Please open the Jenkins instance running on the [10.100.192.200:8080](10.100.192.200:8080) address. Click on the *New Item* link located in the left-hand menu, type *my-first-pipeline*, select *Pipeline* from the the of available job types, and, finally, click the OK button. You will be redirected to the *my-first-pipeline* configuration screen. You will notice that there are no options to add build steps or post-build actions. Apart from some common options like build parameters and the ability to run the job periodically, the whole configuration consists of a single Pipeline definition.

The Node Step
=============

The first Pipeline step we'll learn is `node`. Please type the following in the *Pipeline Script* field.

```ruby
node("build") {
}
```

Go ahead and save the job by clicking the *Apply* button at the bottom of the screen.

At this moment you might be worried that you'll have to learn yet another language. While that is true on the long run, you will be able to start creating Pipeline jobs right away by leveraging the *Snippet Generator*. It is an easy and convenient way to explore available *Pipeline* modules and generate snippets that can be copied into the *Pipeline Script*.

You'll find the *Snippet Generator* checkbox below the script. After selecting it, you will see the list of DSL steps. After selecting one of them, you will be presented with a list of parameters that can be used with the step. Fill them in and click the *Generate Groovy* button. A snippet will appear and the only thing left is to copy and paste it into the Script field above.

Now let us go back to the script we started. *Node* is a step that schedules a task to run by adding it to the Jenkins build queue. As soon as an executor slot is available on a node (the Jenkins master, or a slave), the task is run on that node. A node also allocates a workspace (file directory) on that node for the duration of the task (more on this later). The argument inside brackets can be used to define the name of the node or label. In this case, the code inside this node will run only on those named or with the label *build*.

The Git Step
============

The *git* step performs a clone from the specified repository. We'll use it to get the latest code from the repository cloudbees/training-books-ms. Please note that the git step is intelligent enough to know whether the code should be cloned or pulled from the repository.



TODO: Run the job



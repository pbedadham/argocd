# Best Practices

**Table of Contents**

- [Introduction](#introduction)
- [Single Repo Per Project](#use-a-single-git-repository-per-project)
- [Multiple environments](#use-argocd-to-deploy-your-application-to-multiple-environments)
- [Environment-specific configuration](#create-an-environment-specific-configuration-for-each-environment)
- [Helm charts](#use-helm-charts-with-argocd)
- [Avoid sync policy in production](#avoid-using-argocd’s-sync-policy-in-production)
- [Rollouts instead of sync policy](#use-argo-rollouts-instead-of-argocd’s-sync-policy)


# Introduction
Here are some best practices for large scale and production deployments:


# Use a single Git repository per project
When you have multiple Git repositories for a single project, it can be difficult to keep track of which changes need to be deployed to which environment. This can lead to confusion and errors when trying to deploy your code.

By using a single Git repository per project, you can easily track which changes need to be deployed to which environment. This will make your life much easier when it comes time to deploy your code.

# Use ArgoCD to deploy your application to multiple environments
When you’re developing an application, it’s important to have a development, staging, and production environment. This allows you to test your code in a safe environment before it goes live.

ArgoCD makes it easy to deploy your code to multiple environments. Simply create a new Git branch for each environment, and ArgoCD will automatically deploy your code to that environment.

This is a great way to ensure that your code works in all environments, and it also makes it easy to rollback changes if something goes wrong.

# Create an environment-specific configuration for each environment
When you have a single, shared configuration file for all environments, it’s very easy to accidentally make a change that gets deployed to production when you meant to deploy it to staging. This can cause major problems, and is easily avoided by using a separate configuration file for each environment.

It’s also a good idea to use ArgoCD’s “promote” command to promote changes from one environment to the next, rather than making changes directly in the target environment. This way, you can test changes in lower environments before they get deployed to production.


# Use Helm charts with ArgoCD
Helm charts are a great way to package and deploy applications, and they work well with ArgoCD. ArgoCD can deploy Helm charts from any Git repository, and it will automatically generate the necessary manifests for the chart.

This is a great way to get started with ArgoCD, and it can save you a lot of time when deploying applications.

# Avoid using ArgoCD’s sync policy in production
The sync policy is a powerful feature that allows you to automatically sync your local changes with the remote cluster. However, this feature is not meant to be used in production, as it can lead to data loss and other unexpected side effects.

If you need to use the sync policy in production, make sure to understand the risks involved and take appropriate precautions, such as making regular backups of your data.

# Use Argo Rollouts instead of ArgoCD’s sync policy
ArgoCD’s sync policy is very powerful, but it can also be very dangerous. The sync policy allows you to specify that ArgoCD should automatically sync your changes to the server, without any manual intervention. This is great for making sure that your changes are always up-to-date, but it can also lead to problems if you make a mistake in your configuration.

For example, suppose you accidentally delete a critical file from your repository. If you have the sync policy enabled, ArgoCD will automatically delete the file from the server, without any warning. This can obviously lead to serious problems.

The Argo Rollouts feature is designed to prevent this type of problem. With Argo Rollouts, you can specify that ArgoCD should create a new rollout whenever you make a change to your configuration. This gives you a chance to review the changes before they are applied to the server, and it also allows you to roll back changes if something goes wrong.

Overall, we believe that the Argo Rollouts feature is a much safer way to use ArgoCD, and we recommend that you use it instead of the sync policy.

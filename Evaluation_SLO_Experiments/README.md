# How to recreate the SLO experiments

When you want to recreate the SLO experiment runs, you first have to get a running setup of the MiSArch and the MiSArch Experiment Tool, preferably in a similar hardware surrounding and with Kubernetes, otherwise you can try with a local Docker setup.

## Setup MiSArch as System under Test
The CERES and MiSArch can be installed either in Kubernetes or locally in Docker.

### Install MiSArch incl. MiSArch Experiment Tool in a local Docker environment
1. Clone the [MiSArch Docker Repository](https://github.com/misarch/infrastructure-docker)
2. Execute `docker-compose up -d` in the base directory and wait for 5 minutes.
3. Now everything should be set up and there should be test data in the shop, which is available under `http://localhost:4000`. Log in with the username `gatling` and password `123`.
4. If no test data is available, check the [testdata](https://github.com/misarch/testdata) project and the official [MiSArch documentation](https://misarch.github.io/docs/docs/user-manuals/how-to-use-frontend) and create it manually, which is the same process as described below for Kubernetes.

### Install MiSArch incl. MiSArch Experiment Tool in Kubernetes
1. Checkout the [MiSArch Kubernetes Repository](https://github.com/misarch/infrastructure-k8s)
2. Make sure you have your target Kubernetes cluster configured in your local `kube` config.
3. Install [Terraform](https://developer.hashicorp.com/terraform) and then execute `terraform apply` in the base directory of the repository.
4. After successfull installation, check if everything runs as expected and is installed in the `MiSArch` namespace of your cluster.
5. Create three port-forwards to services in the cluster, (1) to the `misarch-gateway` service, (2) to the `keycloak-service` and (3) to the `misarch-frontend`.
    ```bash
    kubectl -n misarch port-forward svc/misarch-frontend 4000:80
    kubectl -n misarch port-forward svc/keycloak 8081:80
    kubectl -n misarch port-forward svc/misarch-gateway 8080:8080
    ```
6. Open the `Keyclaok` admin console in your browser under `http://localhost:8081` and setup an admin password for the admin console and login to the admin console.
7. Open the frontend under `http://localhost:4000` and click on `login` and then on `register`.
8. Create a user with the username `gatling` and the password `123`.
9. Jump back to the Keycloak admin console and modify the user roles to be admin, employee, and buyer as described in the [MiSArch documentation](https://misarch.github.io/docs/docs/user-manuals/how-to-use-frontend#user-management).
10. Clone the [testdata](https://github.com/misarch/testdata) project.
11. Set the following variables in the `get_token.sh` script.
    ```bash
    KEYCLOAK_URL="http://localhost:8081/keycloak"
    REALM="Misarch"
    ADMIN_USER="admin"
    ADMIN_PASS="admin"
    ADMIN_CLIENT_ID="admin-cli"
    CLIENT_ID="frontend"
    GATLING_USERNAME="gatling"
    GATLING_PASSWORD="123"
    GRANT_TYPE="password"
    ```
12. Modify the `create-test-data.sh` script a little and set the `GRAPHQL_ENDPOINT` variable, to the url of the port-forward to the `misarch-gateway` component in the cluster (e.g., `http://localhost:8080`).
13. Execute `create-test-data.sh`.

Now your MiSArch should be ready, you can verify this by setting opening the frontend under `http://localhost:4000` and check if you can login and buy a CD.

## Setup an Experiment in the MiSArch Experiment Tool

We run two different experiment setups for the SLO generation:
- An experiment configuration with static load and only the `buyProcessScenario`.
- A realistic configuration with realistic load and both the `buyProcessScenario` and the `abortedBuyProcessScenario`.

### Static Experiment Configuration
To get a matching static experiment configuration do the following steps.

1. Open the MiSArch Experiment Tool (in Docker under `http://localhost:5173/frontend`, in Kubernetes create a port-forward to `svc/misarch-experiment-executor-frontend`).
2. Create a new experiment which is a `Scalabilty` experiment with `300s` duration and the default factor.
3. Modify the load by clicking the 'gear'-button and configure the load to be from duration `0` to duration `299` to be 1/2/3/5/10/15 (depending of which run you want to recreate).
4. Add a warm-up time of 10 users for 45s by clicking the burger-menu afterwards (no need to hit apply or save).
5. Click `Execute` and wait for the execution of the experiment to finish.
6. Analyze the results in the dashboard which should be presented to you in a pop-up. (If not search in the Grafana UI for your test-uuid as dashboard name).

### Realistic Experiment Configuration
To get a matching realistic experiment configuration do the following steps.

1. Open the MiSArch Experiment Tool (in Docker under `http://localhost:5173`, in Kubernetes create a port-forward to `svc/misarch-experiment-executor-frontend`).
2. Create a new experiment which is a `Realistic` experiment with `1800s` duration and the default amount of maximum arriving users..
4. Add a warm-up time of 10 users for 45s by clicking the burger-menu afterwards (no need to hit apply or save).
5. Click `Execute` and wait for the execution of the experiment to finish.
6. Analyze the results in the dashboard which should be presented to you in a pop-up. (If not search in the Grafana UI for your test-uuid as dashboard name).
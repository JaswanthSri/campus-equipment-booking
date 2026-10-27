# Campus Equipment Booking Service
Java 17 / Spring Boot REST API for a CI/CD lab assessment.

## Build and test
`mvn clean test`

## Run
`mvn spring-boot:run` — port 8080.

## Endpoints
- `GET /api/equipment`
- `GET /api/equipment/{id}`
- `POST /api/bookings`
- `GET /api/bookings/{id}`
- `POST /api/bookings/{id}/cancel`
- `GET /actuator/health`

Inventory is in memory and resets on restart. No database is required.

## Maven Build & Test Instructions
```bash
mvn clean test
mvn clean package

## Docker Build & Container Verification
'''docker build -t campus-equipment-booking:set-c .
docker run -d -p 8080:8080 --name equipment-app campus-equipment-booking:set-c
curl http://localhost:8080/actuator/health

## Minikube / Kubernetes Deployment & Scaling
'''minikube image load campus-equipment-booking:set-c
kubectl apply -f deployment.yaml
kubectl scale deployment campus-equipment-deployment --replicas=2
kubectl get pods -l app=campus-equipment

## Health Check URL & Access Method

Access the service health check through Minikube NodePort:
'''curl $(minikube service campus-equipment-service --url)/actuator/health

Output: {"status":"UP","groups":["liveness","readiness"]}

## Assigned Change Request & Defect Explanation (Set C)


Booking Capacity Defect Fix (Task 3):

        Defect: The boundary check allowed bookings when quantity > e.getAvailableQuantity() + 1, letting requests exceed stock by 1 unit.

        Correction: Updated condition in BookingService.java to strictly quantity > e.getAvailableQuantity().

        Verification: Unit tests exactCapacityBookingAllowed and offByOneOverbookingRejected verify that exact capacity is accepted and overbooking requests are rejected without modifying inventory.

    Invalid Identifier Handling (Task 4):

        Requests for unknown equipment IDs throw a NoSuchElementException in BookingService.java, preventing any booking creation or inventory changes.

        Verified via unit test unknownEquipmentRejected.


---

### Final Git Steps:
Once saved, complete your final git commit on `feature/equipment-lookup-validation`:

```bash
git add README.md deployment.yaml
git commit -m "Task 7: Update README documentation and add Kubernetes manifests"

# Salesforce Financial Account Event Listener

Listener d'événements Salesforce pour les comptes financiers, transactions et cartes. Publie les événements vers Anypoint MQ Topics.

## Description

Cette application écoute les événements Salesforce concernant les comptes financiers, transactions et cartes via des webhooks HTTP. Elle transforme ces événements et les publie vers les Topics Anypoint MQ correspondants.

## Endpoints Webhook

### POST /events/financial-account
Reçoit les événements de compte financier depuis Salesforce et publie vers "Accounts Topic".

### POST /events/transaction
Reçoit les événements de transaction depuis Salesforce et publie vers "Transactions Topic".

### POST /events/card
Reçoit les événements de carte depuis Salesforce et publie vers "Cards Topic".

## Configuration

### Salesforce

1. Configurer les propriétés dans `src/main/resources/config.properties`:

```properties
salesforce.username=your-username
salesforce.password=your-password
salesforce.securityToken=your-security-token
```

2. Configurer les webhooks dans Salesforce pour pointer vers :
   - `http://your-mule-app-url/events/financial-account`
   - `http://your-mule-app-url/events/transaction`
   - `http://your-mule-app-url/events/card`

### Anypoint MQ

1. Configurer les propriétés MQ :

```properties
anypoint.mq.url=https://mq-us-east-1.anypoint.mulesoft.com/api/v1/
anypoint.mq.clientId=your-mq-client-id
anypoint.mq.clientSecret=your-mq-client-secret
```

2. Créer les Topics dans Anypoint Platform :
   - **Accounts Topic**
   - **Transactions Topic**
   - **Cards Topic**

3. Créer les Queues correspondantes (qui consomment depuis les Topics) :
   - **Accounts Queue** → consomme depuis "Accounts Topic"
   - **Transactions Queue** → consomme depuis "Transactions Topic"
   - **Cards Queue** → consomme depuis "Cards Topic"

## Architecture Technique

### Flows

- `financial-account-event-listener`: Écoute les événements de compte financier
- `transaction-event-listener`: Écoute les événements de transaction
- `card-event-listener`: Écoute les événements de carte

### Publication MQ

Chaque flow transforme l'événement Salesforce au format requis et publie vers le Topic correspondant via `anypoint-mq:publish`.

## Port

- **Port HTTP**: 8084

## Dépendances

- Anypoint MQ Connector (5.4.3)
- Salesforce Connector (11.8.0)
- HTTP Connector (1.9.2)


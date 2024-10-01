# Transactional Outbox Pattern - Trigger-based Action System

This project demonstrates the implementation of the **Transactional Outbox Pattern** using **Prisma** and **Kafka**. The system triggers actions, such as invoking webhooks, whenever certain events occur. It ensures reliability by separating the trigger event from the action execution using an outbox table and Kafka for message processing.

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [How It Works](#how-it-works)
- [Endpoints](#endpoints)


## Introduction

This project implements the **Transactional Outbox Pattern**, which decouples the trigger event from the action (e.g., webhook calls) in a reliable manner. When an event is triggered, it's logged in an outbox table, which is processed asynchronously by Kafka to execute the corresponding action.

## Features

- **Webhook Trigger**: Stores a webhook trigger in the database.
- **Outbox Table**: Uses a transactional outbox to ensure reliability between trigger and execution.
- **Kafka Integration**: Processes outbox events via Kafka.
- **Prisma ORM**: For database management.
- **Express Framework**: For webhook endpoints.

## Tech Stack

- **Node.js**
- **Express**
- **Prisma ORM**
- **KafkaJS**
- **PostgreSQL/MySQL** (replace based on your DB)
- **Docker** (for Kafka, optional)

## How It Works

1. **Webhook Trigger**: The `/hooks/catch/:userId/:zapId` endpoint stores a trigger event in the `zapRun` and `zapRunOutbox` tables.
2. **Transactional Outbox**: Both the trigger event and the outbox entry are stored within the same transaction.
3. **Kafka Processor**: A background worker reads pending entries from the outbox table and sends the event to a Kafka topic. The outbox entry is deleted once it's successfully processed.


## Endpoints

### POST `/hooks/catch/:userId/:zapId`

- **Description**: Receives a webhook trigger and stores it in the database.
- **Parameters**:
  - `userId`: The ID of the user sending the webhook.
  - `zapId`: The ID of the zap (trigger event).
- **Body**: Accepts a JSON payload with metadata.
- **Response**:
  ```json
  {
    "message": "Webhook received"
  }
curl -X POST http://localhost:3002/hooks/catch/123/456 -H "Content-Type: application/json" -d '{"someData": "value"}'



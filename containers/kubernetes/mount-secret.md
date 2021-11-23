# Using Kubernetes secrets

## Accessing secrets

Secrets can be accessed as environment variables or as files.

### As environment variables

```yaml
    containers:
      - env:
        - name: ENV_VAR_NAME
          valueFrom:
            secretKeyRef:
              name: secret-name
              key: key-in-secret
```

### As files

Set up the container mounts within the containers.

```yaml
      containers:
      - volumeMounts:
        - name: volume-name
          mountPath: /data/mount-path
          readOnly: true
```

Then create the volume. This config is a sibling to the containers config.

```yaml
      volumes:
      - name: volume-name
        secret:
          secretName: asset-sso
```

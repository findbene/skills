# Custom n8n Nodes Reference

## When to Build a Custom Node
- An API you need doesn't have an n8n community node
- You need complex business logic that's hard to express in Function nodes
- You're packaging a reusable integration for distribution

## Project Structure
```
my-n8n-node/
  package.json
  tsconfig.json
  nodes/
    MyNode/
      MyNode.node.ts
      mynode.svg            (icon -- 60x60px, SVG)
  credentials/
    MyNodeApi.credentials.ts
```

## package.json
```json
{
  "name": "n8n-nodes-mynode",
  "version": "0.1.0",
  "description": "n8n node for My API",
  "keywords": ["n8n-community-node-package"],
  "license": "MIT",
  "main": "index.js",
  "scripts": {
    "build": "tsc && gulp build:icons",
    "dev": "tsc --watch"
  },
  "n8n": {
    "n8nNodesApiVersion": 1,
    "credentials": ["dist/credentials/MyNodeApi.credentials.js"],
    "nodes": ["dist/nodes/MyNode/MyNode.node.js"]
  },
  "devDependencies": {
    "@types/node": "^18.0.0",
    "n8n-workflow": "*",
    "typescript": "^5.0.0"
  }
}
```

## Node Implementation (TypeScript)
```typescript
import {
    IExecuteFunctions,
    INodeExecutionData,
    INodeType,
    INodeTypeDescription,
    NodeOperationError,
} from 'n8n-workflow';

export class MyNode implements INodeType {
    description: INodeTypeDescription = {
        displayName: 'My Node',
        name: 'myNode',
        icon: 'file:mynode.svg',
        group: ['transform'],
        version: 1,
        description: 'Interact with My API',
        defaults: {
            name: 'My Node',
        },
        inputs: ['main'],
        outputs: ['main'],
        credentials: [
            {
                name: 'myNodeApi',
                required: true,
            },
        ],
        properties: [
            {
                displayName: 'Operation',
                name: 'operation',
                type: 'options',
                noDataExpression: true,
                options: [
                    {
                        name: 'Get Item',
                        value: 'getItem',
                        description: 'Retrieve a single item',
                        action: 'Get item',
                    },
                    {
                        name: 'Create Item',
                        value: 'createItem',
                        description: 'Create a new item',
                        action: 'Create item',
                    },
                ],
                default: 'getItem',
            },
            {
                displayName: 'Item ID',
                name: 'itemId',
                type: 'string',
                required: true,
                displayOptions: {
                    show: {
                        operation: ['getItem'],
                    },
                },
                default: '',
                description: 'The ID of the item to retrieve',
            },
        ],
    };

    async execute(this: IExecuteFunctions): Promise<INodeExecutionData[][]> {
        const items = this.getInputData();
        const returnData: INodeExecutionData[] = [];

        const credentials = await this.getCredentials('myNodeApi');
        const baseUrl = credentials.baseUrl as string;
        const apiKey = credentials.apiKey as string;

        for (let i = 0; i < items.length; i++) {
            const operation = this.getNodeParameter('operation', i) as string;

            if (operation === 'getItem') {
                const itemId = this.getNodeParameter('itemId', i) as string;

                const response = await this.helpers.request({
                    method: 'GET',
                    url: `${baseUrl}/items/${itemId}`,
                    headers: {
                        'Authorization': `Bearer ${apiKey}`,
                    },
                    json: true,
                });

                returnData.push({ json: response });
            } else if (operation === 'createItem') {
                throw new NodeOperationError(
                    this.getNode(),
                    'Create Item not yet implemented',
                    { itemIndex: i },
                );
            }
        }

        return [returnData];
    }
}
```

## Credentials Implementation
```typescript
import {
    ICredentialType,
    INodeProperties,
} from 'n8n-workflow';

export class MyNodeApi implements ICredentialType {
    name = 'myNodeApi';
    displayName = 'My Node API';
    documentationUrl = 'https://docs.myapi.com/auth';
    properties: INodeProperties[] = [
        {
            displayName: 'Base URL',
            name: 'baseUrl',
            type: 'string',
            default: 'https://api.myservice.com',
        },
        {
            displayName: 'API Key',
            name: 'apiKey',
            type: 'string',
            typeOptions: {
                password: true,  // masks the value in UI
            },
            default: '',
        },
    ];
}
```

## Installing Custom Nodes in Self-Hosted n8n
```bash
# Via npm (Docker)
docker exec -it n8n npm install n8n-nodes-mynode

# Via volume mount (development)
# In docker-compose.yml:
volumes:
  - ~/.n8n/custom:/home/node/.n8n/custom

# Place compiled node in:
# ~/.n8n/custom/node_modules/n8n-nodes-mynode/
```

## Testing Custom Nodes Locally
```bash
# Link your node to n8n's node_modules
cd my-n8n-node
npm run build
npm link

cd ~/.n8n
npm link n8n-nodes-mynode

# Start n8n -- your node will appear in the node palette
npx n8n start
```

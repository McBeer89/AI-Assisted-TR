# Reference: Example Detection Data Models (Arrows.app JSON)

A curated cross-section of published DDMs, chosen to show structurally
different cases - not every DDM. Each block is exact Arrows.app JSON; paste
any block into https://arrows.app to see it rendered. Conventions: black
arrows = flow; red arrows (#f44e3b) = the active path in a per-procedure
export; grey nodes (#cccccc) = an operation that normally occurs but is
skipped in this procedure; grey arrows (#999999) = secondary/optional flow.


## ddm_trr0016_win.json

Shared-pipeline master, compact (8 operations). One procedure's path is shown in red over the shared graph - the per-procedure export pattern.

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 4,
    "border-color": "#000000",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 16,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 2,
    "label-font-size": 14,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 5,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 16,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n0",
      "position": {
        "x": 75,
        "y": 50
      },
      "caption": "Start Process",
      "labels": [
        "CS PR2",
        "Windows 4688"
      ],
      "properties": {
        "Process": "mshta.exe"
      },
      "style": {
        "border-color": "#000000",
        "caption-font-size": 16,
        "label-position": "outside",
        "label-border-width": 2,
        "label-font-size": 16
      }
    },
    {
      "id": "n1",
      "position": {
        "x": 507.99870179421924,
        "y": 50
      },
      "caption": "Load DLL",
      "labels": [],
      "properties": {
        "Name": "mshtml.dll"
      },
      "style": {
        "border-color": "#000000",
        "caption-font-size": 16
      }
    },
    {
      "id": "n2",
      "position": {
        "x": 679.9987017942192,
        "y": 50
      },
      "caption": "Call Function",
      "labels": [],
      "properties": {
        "Name": "RunHTMLApplication"
      },
      "style": {
        "border-color": "#000000",
        "caption-font-size": 16
      }
    },
    {
      "id": "n3",
      "position": {
        "x": 567.7027418345734,
        "y": 384.98343844251747
      },
      "caption": "Find Protocol Handler",
      "labels": [],
      "properties": {
        "Protocol": "varies"
      },
      "style": {
        "caption-font-size": 16,
        "border-color": "#000000"
      }
    },
    {
      "id": "n4",
      "position": {
        "x": 291.4993508971096,
        "y": 50
      },
      "caption": "Query Registry",
      "labels": [
        "Windows 4663 (SACL)",
        "Sysmon 12"
      ],
      "properties": {
        "Key": "COM Object GUID"
      },
      "style": {
        "caption-font-size": 16,
        "border-color": "#000000",
        "label-position": "outside",
        "label-border-width": 2,
        "label-font-size": 16
      }
    },
    {
      "id": "n5",
      "position": {
        "x": 921.0075488412574,
        "y": 384.98343844251747
      },
      "caption": "Run Application",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#000000",
        "caption-font-size": 16
      }
    },
    {
      "id": "n6",
      "position": {
        "x": 75,
        "y": 212.94228960296093
      },
      "caption": "Start Process",
      "labels": [
        "CS PR2",
        "Windows 4688"
      ],
      "properties": {
        "Process": "rundll32.exe"
      },
      "style": {
        "border-color": "#000000",
        "caption-font-size": 16,
        "caption-position": "inside",
        "label-position": "outside",
        "label-font-size": 16,
        "label-border-width": 2
      }
    },
    {
      "id": "n7",
      "position": {
        "x": 752.4497121689539,
        "y": 258.3350756900909
      },
      "caption": "Call API",
      "labels": [],
      "properties": {
        "API": "CreateFile"
      },
      "style": {
        "caption-font-size": 16,
        "caption-position": "inside",
        "label-position": "outside",
        "label-font-size": 16
      }
    }
  ],
  "relationships": [
    {
      "id": "n0",
      "fromId": "n1",
      "toId": "n2",
      "type": "",
      "properties": {},
      "style": {
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n1",
      "fromId": "n2",
      "toId": "n3",
      "type": "if Inline",
      "properties": {},
      "style": {
        "detail-orientation": "horizontal",
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n2",
      "fromId": "n0",
      "toId": "n4",
      "type": "",
      "properties": {},
      "style": {
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n3",
      "fromId": "n4",
      "toId": "n1",
      "type": "",
      "properties": {},
      "style": {
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n4",
      "fromId": "n3",
      "toId": "n5",
      "type": "",
      "properties": {},
      "style": {
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n5",
      "fromId": "n6",
      "toId": "n1",
      "type": "",
      "properties": {},
      "style": {
        "arrow-color": "#000000"
      }
    },
    {
      "id": "n6",
      "fromId": "n2",
      "toId": "n7",
      "type": "if File",
      "properties": {},
      "style": {
        "detail-orientation": "horizontal",
        "detail-position": "inline",
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n7",
      "fromId": "n7",
      "toId": "n5",
      "type": "",
      "properties": {},
      "style": {
        "arrow-color": "#f44e3b"
      }
    }
  ]
}
```


## trr0029_a.json

Per-procedure export (procedure A). Same full 14-operation graph as trr0029_b, with procedure A's active path in red.

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 3,
    "border-color": "#68bc00",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 14,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 1,
    "label-font-size": 12,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 3,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 12,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n0",
      "position": {
        "x": 965.4979856764037,
        "y": 181.11811219932508
      },
      "caption": "Send Network Request",
      "labels": [],
      "properties": {
        "Host": "Any",
        "User": "Any",
        "Kerberos Type": "TGS-REQ",
        "KDCOptions": "ENC-TKT-IN-SKEY (U2U)",
        "AdditionalTickets": "TGT (from AS-REP)"
      },
      "style": {
        "node-color": "#ffffff",
        "border-color": "#68bc00",
        "property-font-size": 12,
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2
      }
    },
    {
      "id": "n1",
      "position": {
        "x": 1184.0428455177707,
        "y": 181.11811219932525
      },
      "caption": "Receive Network Request",
      "labels": [
        "Event 4769",
        "CS ActiveDirectoryServiceAccessRequest"
      ],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "TGS-REQ",
        "KDCOptions": "ENC-TKT-IN-SKEY (U2U)",
        "AdditionalTickets": "TGT (from AS-REP)"
      },
      "style": {
        "border-color": "#0062b1",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 1,
        "property-font-size": 12
      }
    },
    {
      "id": "n4",
      "position": {
        "x": 1402.587705359138,
        "y": 181.11811219932508
      },
      "caption": "Send Network Response",
      "labels": [],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "TGS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#0062b1",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n7",
      "position": {
        "x": 1621.1325652005053,
        "y": 181.11811219932497
      },
      "caption": "Receive Network Request",
      "labels": [],
      "properties": {
        "Kerberos Type": "TGS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n8",
      "position": {
        "x": -57.741100869385065,
        "y": 181.11811219932525
      },
      "caption": "Send Network Request",
      "labels": [],
      "properties": {
        "Host": "Any",
        "User": "Any",
        "Kerberos Type": "AS-REQ",
        "padata": "PA-PK-AS-REQ (Type 16)"
      },
      "style": {
        "node-color": "#ffffff",
        "border-color": "#68bc00",
        "property-font-size": 12,
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2
      }
    },
    {
      "id": "n9",
      "position": {
        "x": 160.80375897198186,
        "y": 181.11811219932497
      },
      "caption": "Receive Network Request",
      "labels": [
        "Event 4768",
        "CS ActiveDirectoryAuthentication"
      ],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "AS-REQ",
        "padata": "PA-PK-AS-REQ (Type 16)"
      },
      "style": {
        "border-color": "#0062b1",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 1,
        "property-font-size": 12
      }
    },
    {
      "id": "n17",
      "position": {
        "x": 528.4082659936696,
        "y": 181.11811219932514
      },
      "caption": "Send Network Response",
      "labels": [],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "AS-REP",
        "Contains": "PAC_CREDENTIAL_INFO"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#0062b1",
        "label-font-size": 12,
        "property-font-size": 12,
        "node-color": "#ffffff"
      }
    },
    {
      "id": "n20",
      "position": {
        "x": 746.9531258350366,
        "y": 181.1181121993251
      },
      "caption": "Receive Network Response",
      "labels": [],
      "properties": {
        "Kerberos Type": "AS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n21",
      "position": {
        "x": 1839.6774250418723,
        "y": 181.11811219932525
      },
      "caption": "Decrypt message",
      "style": {},
      "labels": [],
      "properties": {
        "Kerberos Type": "TGS-REP",
        "Contains": "PAC_CREDENTIAL_INFO"
      }
    },
    {
      "id": "n22",
      "position": {
        "x": 344.60601248282575,
        "y": 181.11811219932505
      },
      "caption": "Validate Certificate",
      "style": {
        "border-color": "#0062b1"
      },
      "labels": [],
      "properties": {}
    },
    {
      "id": "n23",
      "position": {
        "x": 398.7795782735502,
        "y": 479.8883165747842
      },
      "caption": "Query Active Directory Data",
      "style": {
        "border-color": "#0062b1"
      },
      "labels": [],
      "properties": {
        "Attribute": "MSDS-KeyCredentialLink"
      }
    },
    {
      "id": "n24",
      "position": {
        "x": 253.235418511508,
        "y": 400.74768279859495
      },
      "caption": "Verify Certificate Trust Chain",
      "style": {
        "border-color": "#0062b1"
      },
      "labels": [],
      "properties": {}
    },
    {
      "id": "n25",
      "position": {
        "x": -328.3432933761764,
        "y": 130.5387913149396
      },
      "caption": "Acquire Certificate",
      "labels": [],
      "properties": {
        "EKU": "Smart Card Logon"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n26",
      "position": {
        "x": -328.3432933761764,
        "y": 256.3945999826505
      },
      "caption": "Write AD Attribute",
      "labels": [
        "Event 5136 (SACL)"
      ],
      "properties": {
        "Attribute": "MSDS-KeyCredentialLink"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    }
  ],
  "relationships": [
    {
      "id": "n3",
      "fromId": "n1",
      "toId": "n4",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12,
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n19",
      "fromId": "n20",
      "toId": "n0",
      "type": "",
      "properties": {},
      "style": {
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n20",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n8",
      "toId": "n9"
    },
    {
      "id": "n21",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n17",
      "toId": "n20"
    },
    {
      "id": "n22",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n0",
      "toId": "n1"
    },
    {
      "id": "n23",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n4",
      "toId": "n7"
    },
    {
      "id": "n24",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n7",
      "toId": "n21"
    },
    {
      "id": "n25",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n9",
      "toId": "n22"
    },
    {
      "id": "n26",
      "type": "key trust",
      "style": {
        "detail-orientation": "horizontal",
        "arrow-color": "#000000"
      },
      "properties": {},
      "fromId": "n22",
      "toId": "n23"
    },
    {
      "id": "n27",
      "type": "certificate trust",
      "style": {
        "detail-orientation": "horizontal",
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n22",
      "toId": "n24"
    },
    {
      "id": "n28",
      "type": "",
      "style": {
        "arrow-color": "#000000"
      },
      "properties": {},
      "fromId": "n23",
      "toId": "n17"
    },
    {
      "id": "n29",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n24",
      "toId": "n17"
    },
    {
      "id": "n30",
      "type": "certificate trust",
      "style": {
        "detail-orientation": "horizontal",
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n25",
      "toId": "n8"
    },
    {
      "id": "n31",
      "type": "key trust",
      "style": {
        "detail-orientation": "horizontal",
        "arrow-color": "#000000"
      },
      "properties": {},
      "fromId": "n26",
      "toId": "n8"
    }
  ]
}
```


## trr0029_b.json

Per-procedure export (procedure B) - the SAME graph as trr0029_a, reddening procedure B's path instead. The pair shows how shared-pipeline procedures are exported: keep the whole graph, highlight one path.

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 3,
    "border-color": "#68bc00",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 14,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 1,
    "label-font-size": 12,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 3,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 12,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n0",
      "position": {
        "x": 965.4979856764037,
        "y": 181.11811219932508
      },
      "caption": "Send Network Request",
      "labels": [],
      "properties": {
        "Host": "Any",
        "User": "Any",
        "Kerberos Type": "TGS-REQ",
        "KDCOptions": "ENC-TKT-IN-SKEY (U2U)",
        "AdditionalTickets": "TGT (from AS-REP)"
      },
      "style": {
        "node-color": "#ffffff",
        "border-color": "#68bc00",
        "property-font-size": 12,
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2
      }
    },
    {
      "id": "n1",
      "position": {
        "x": 1184.0428455177707,
        "y": 181.11811219932525
      },
      "caption": "Receive Network Request",
      "labels": [
        "Event 4769",
        "CS ActiveDirectoryServiceAccessRequest"
      ],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "TGS-REQ",
        "KDCOptions": "ENC-TKT-IN-SKEY (U2U)",
        "AdditionalTickets": "TGT (from AS-REP)"
      },
      "style": {
        "border-color": "#0062b1",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 1,
        "property-font-size": 12
      }
    },
    {
      "id": "n4",
      "position": {
        "x": 1402.587705359138,
        "y": 181.11811219932508
      },
      "caption": "Send Network Response",
      "labels": [],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "TGS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#0062b1",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n7",
      "position": {
        "x": 1621.1325652005053,
        "y": 181.11811219932497
      },
      "caption": "Receive Network Request",
      "labels": [],
      "properties": {
        "Kerberos Type": "TGS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n8",
      "position": {
        "x": -57.741100869385065,
        "y": 181.11811219932525
      },
      "caption": "Send Network Request",
      "labels": [],
      "properties": {
        "Host": "Any",
        "User": "Any",
        "Kerberos Type": "AS-REQ",
        "padata": "PA-PK-AS-REQ (Type 16)"
      },
      "style": {
        "node-color": "#ffffff",
        "border-color": "#68bc00",
        "property-font-size": 12,
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2
      }
    },
    {
      "id": "n9",
      "position": {
        "x": 160.80375897198186,
        "y": 181.11811219932497
      },
      "caption": "Receive Network Request",
      "labels": [
        "Event 4768",
        "CS ActiveDirectoryAuthentication"
      ],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "AS-REQ",
        "padata": "PA-PK-AS-REQ (Type 16)"
      },
      "style": {
        "border-color": "#0062b1",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 1,
        "property-font-size": 12
      }
    },
    {
      "id": "n17",
      "position": {
        "x": 528.4082659936696,
        "y": 181.11811219932514
      },
      "caption": "Send Network Response",
      "labels": [],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "AS-REP",
        "Contains": "PAC_CREDENTIAL_INFO"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#0062b1",
        "label-font-size": 12,
        "property-font-size": 12,
        "node-color": "#ffffff"
      }
    },
    {
      "id": "n20",
      "position": {
        "x": 746.9531258350366,
        "y": 181.1181121993251
      },
      "caption": "Receive Network Response",
      "labels": [],
      "properties": {
        "Kerberos Type": "AS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n21",
      "position": {
        "x": 1839.6774250418723,
        "y": 181.11811219932525
      },
      "caption": "Decrypt message",
      "style": {},
      "labels": [],
      "properties": {
        "Kerberos Type": "TGS-REP",
        "Contains": "PAC_CREDENTIAL_INFO"
      }
    },
    {
      "id": "n22",
      "position": {
        "x": 344.60601248282575,
        "y": 181.11811219932505
      },
      "caption": "Validate Certificate",
      "style": {
        "border-color": "#0062b1"
      },
      "labels": [],
      "properties": {}
    },
    {
      "id": "n23",
      "position": {
        "x": 398.7795782735502,
        "y": 479.8883165747842
      },
      "caption": "Query Active Directory Data",
      "style": {
        "border-color": "#0062b1"
      },
      "labels": [],
      "properties": {
        "Attribute": "MSDS-KeyCredentialLink"
      }
    },
    {
      "id": "n24",
      "position": {
        "x": 253.235418511508,
        "y": 400.74768279859495
      },
      "caption": "Verify Certificate Trust Chain",
      "style": {
        "border-color": "#0062b1"
      },
      "labels": [],
      "properties": {}
    },
    {
      "id": "n25",
      "position": {
        "x": -328.3432933761764,
        "y": 130.5387913149396
      },
      "caption": "Acquire Certificate",
      "labels": [],
      "properties": {
        "EKU": "Smart Card Logon"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n26",
      "position": {
        "x": -328.3432933761764,
        "y": 256.3945999826505
      },
      "caption": "Write AD Attribute",
      "labels": [
        "Event 5136 (SACL)"
      ],
      "properties": {
        "Attribute": "MSDS-KeyCredentialLink"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    }
  ],
  "relationships": [
    {
      "id": "n3",
      "fromId": "n1",
      "toId": "n4",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12,
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n19",
      "fromId": "n20",
      "toId": "n0",
      "type": "",
      "properties": {},
      "style": {
        "arrow-color": "#f44e3b"
      }
    },
    {
      "id": "n20",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n8",
      "toId": "n9"
    },
    {
      "id": "n21",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n17",
      "toId": "n20"
    },
    {
      "id": "n22",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n0",
      "toId": "n1"
    },
    {
      "id": "n23",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n4",
      "toId": "n7"
    },
    {
      "id": "n24",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n7",
      "toId": "n21"
    },
    {
      "id": "n25",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n9",
      "toId": "n22"
    },
    {
      "id": "n26",
      "type": "key trust",
      "style": {
        "detail-orientation": "horizontal",
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n22",
      "toId": "n23"
    },
    {
      "id": "n27",
      "type": "certificate trust",
      "style": {
        "detail-orientation": "horizontal",
        "arrow-color": "#000000"
      },
      "properties": {},
      "fromId": "n22",
      "toId": "n24"
    },
    {
      "id": "n28",
      "type": "",
      "style": {
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n23",
      "toId": "n17"
    },
    {
      "id": "n29",
      "type": "",
      "style": {
        "arrow-color": "#000000"
      },
      "properties": {},
      "fromId": "n24",
      "toId": "n17"
    },
    {
      "id": "n30",
      "type": "certificate trust",
      "style": {
        "detail-orientation": "horizontal",
        "arrow-color": "#000000"
      },
      "properties": {},
      "fromId": "n25",
      "toId": "n8"
    },
    {
      "id": "n31",
      "type": "key trust",
      "style": {
        "detail-orientation": "horizontal",
        "arrow-color": "#f44e3b"
      },
      "properties": {},
      "fromId": "n26",
      "toId": "n8"
    }
  ]
}
```


## trr0023_a.json

Independent procedure A - a self-contained chain, all arrows black.

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 2,
    "border-color": "#000000",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 16,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 1,
    "label-font-size": 16,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 5,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 16,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n10",
      "position": {
        "x": 593.0490025457719,
        "y": 132
      },
      "caption": "Call RPC",
      "labels": [],
      "properties": {
        "Process": "any",
        "RPC": "ElfrClearELFA or ElfrClearELFW",
        "OpNum": "12 or 0",
        "Interface": "MS-EVEN",
        "Transport": "\\pipe\\eventlog"
      },
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13,
        "label-font-size": 13
      }
    },
    {
      "id": "n11",
      "position": {
        "x": 711.0490025457719,
        "y": -143.5691082089424
      },
      "caption": "Clear Event Log",
      "labels": [],
      "properties": {
        "Permissions": "Administrator",
        "Process": "any"
      },
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13
      }
    },
    {
      "id": "n12",
      "position": {
        "x": 711.0490025457719,
        "y": 238
      },
      "caption": "Call RPC",
      "labels": [],
      "properties": {
        "Process": "any",
        "RPC": "EvtRpcClearLog",
        "OpNum": "6",
        "Interface": "MS-EVEN6",
        "Transport": "RPC over TCP/IP"
      },
      "style": {
        "border-color": "#68bc00",
        "caption-font-size": 16,
        "property-font-size": 13
      }
    },
    {
      "id": "n13",
      "position": {
        "x": 1001.3891805459289,
        "y": 128.27505278661454
      },
      "caption": "Receive RPC",
      "labels": [
        "CS EventLogCleared",
        "WinEvent 1102"
      ],
      "properties": {
        "Process": "svchost.exe",
        "DLL": "wevtsvc.dll"
      },
      "style": {
        "border-color": "#0062b1",
        "caption-font-size": 16,
        "label-font-size": 13,
        "property-font-size": 13
      }
    },
    {
      "id": "n14",
      "position": {
        "x": 1287.6963197679368,
        "y": -18.85935039494079
      },
      "caption": "Overwrite File",
      "labels": [],
      "properties": {
        "Process": "svchost.exe"
      },
      "style": {
        "border-color": "#0062b1",
        "property-font-size": 13
      }
    },
    {
      "id": "n15",
      "position": {
        "x": 1287.6963197679368,
        "y": 238
      },
      "caption": "Call API",
      "style": {
        "border-color": "#0062b1",
        "property-font-size": 13
      },
      "labels": [],
      "properties": {
        "API": "NtWriteFile",
        "Process": "svchost.exe"
      }
    }
  ],
  "relationships": [
    {
      "id": "n0",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n12",
      "toId": "n13"
    },
    {
      "id": "n1",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n10",
      "toId": "n13"
    },
    {
      "id": "n2",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n13",
      "toId": "n14"
    },
    {
      "id": "n3",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n11",
      "toId": "n10"
    },
    {
      "id": "n4",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n11",
      "toId": "n12"
    },
    {
      "id": "n5",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n14",
      "toId": "n15"
    }
  ]
}
```


## trr0023_b.json

Independent procedure B.

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 2,
    "border-color": "#000000",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 16,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 1,
    "label-font-size": 16,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 5,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 16,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n12",
      "position": {
        "x": 661.1283484115035,
        "y": -128.8593503949408
      },
      "caption": "Terminate Process",
      "labels": [],
      "properties": {
        "Permissions": "Administrator"
      },
      "style": {
        "border-color": "#68bc00",
        "caption-font-size": 16,
        "property-font-size": 13
      }
    },
    {
      "id": "n14",
      "position": {
        "x": 1142.045639450858,
        "y": -124.8593503949408
      },
      "caption": "Modify File",
      "labels": [],
      "properties": {
        "Permissions": "Administrator"
      },
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13
      }
    },
    {
      "id": "n15",
      "position": {
        "x": 1142.045639450858,
        "y": 39.85604098797374
      },
      "caption": "Call API",
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13
      },
      "labels": [],
      "properties": {
        "API": "NtWriteFile"
      }
    },
    {
      "id": "n16",
      "position": {
        "x": 661.1283484115035,
        "y": -287.0033094069671
      },
      "caption": "Release File Lock",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13,
        "label-font-size": 13
      }
    },
    {
      "id": "n17",
      "position": {
        "x": 1268.045639450858,
        "y": -124.8593503949408
      },
      "caption": "Delete File",
      "labels": [],
      "properties": {
        "Permissions": "Administrator"
      },
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13
      }
    },
    {
      "id": "n18",
      "position": {
        "x": 1268.045639450858,
        "y": 39.85604098797374
      },
      "caption": "Call API",
      "labels": [],
      "properties": {
        "API": "NtDeleteFile"
      },
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13
      }
    },
    {
      "id": "n19",
      "position": {
        "x": 737.667347625051,
        "y": 39.85604098797374
      },
      "caption": "Call API",
      "style": {
        "border-color": "#68bc00",
        "caption-font-size": 16,
        "property-font-size": 13
      },
      "labels": [],
      "properties": {
        "API": "NtTerminateProcess",
        "Target": "svchost.exe",
        "Hosted Service": "EventLog"
      }
    },
    {
      "id": "n20",
      "position": {
        "x": 584.5893491979562,
        "y": 39.85604098797374
      },
      "caption": "Call WMI Method",
      "style": {
        "border-color": "#68bc00",
        "caption-font-size": 16,
        "property-font-size": 13
      },
      "labels": [],
      "properties": {
        "Class": "Win32_Process",
        "Method": "Terminate",
        "Target": "svchost.exe",
        "Hosted Service": "EventLog"
      }
    },
    {
      "id": "n21",
      "position": {
        "x": 1214.7263510109515,
        "y": -287.0033094069671
      },
      "caption": "Clear Log",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13,
        "label-font-size": 13
      }
    },
    {
      "id": "n22",
      "position": {
        "x": 937.9273497112274,
        "y": -287.0033094069671
      },
      "caption": "Process Terminates",
      "labels": [
        "CS EndOfProcess",
        "WinEvent 4689"
      ],
      "properties": {
        "Process": "svchost.exe",
        "Hosted Service": "EventLog",
        "Exit Code": "1"
      },
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13,
        "label-font-size": 13
      }
    }
  ],
  "relationships": [
    {
      "id": "n5",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n14",
      "toId": "n15"
    },
    {
      "id": "n7",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n16",
      "toId": "n12"
    },
    {
      "id": "n10",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n17",
      "toId": "n18"
    },
    {
      "id": "n11",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n12",
      "toId": "n19"
    },
    {
      "id": "n12",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n12",
      "toId": "n20"
    },
    {
      "id": "n13",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n21",
      "toId": "n14"
    },
    {
      "id": "n14",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n21",
      "toId": "n17"
    },
    {
      "id": "n15",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n19",
      "toId": "n22"
    },
    {
      "id": "n16",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n20",
      "toId": "n22"
    },
    {
      "id": "n17",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n22",
      "toId": "n21"
    }
  ]
}
```


## trr0023_c.json

Independent procedure C. When procedures share no operations, each export carries only its own chain (contrast with the trr0029 pair).

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 2,
    "border-color": "#000000",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 16,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 1,
    "label-font-size": 16,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 5,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 16,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n22",
      "position": {
        "x": -7478.801180593765,
        "y": -2762.0915311231765
      },
      "caption": "Set Registry Value",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13,
        "label-font-size": 13
      }
    },
    {
      "id": "n23",
      "position": {
        "x": -7226.640046224957,
        "y": -2762.0915311231765
      },
      "caption": "Activity to be Concealed",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13,
        "label-font-size": 13
      }
    },
    {
      "id": "n24",
      "position": {
        "x": -6995.640046224957,
        "y": -2762.0915311231765
      },
      "caption": "Set Registry Value",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13,
        "label-font-size": 13
      }
    },
    {
      "id": "n25",
      "position": {
        "x": -6764.640046224957,
        "y": -2762.0915311231765
      },
      "caption": "Delete File",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#808080",
        "property-font-size": 13,
        "label-font-size": 13
      }
    },
    {
      "id": "n26",
      "position": {
        "x": -7478.801180593765,
        "y": -2572.0915311231765
      },
      "caption": "Call API",
      "labels": [
        "CS RegSystemConfigValueUpdate",
        "WinEvent 4657 (requires SACL)"
      ],
      "properties": {
        "API": "NtSetValueKey ",
        "Key": "HKLM\\SYSTEM\\CurrentControlSet\\Services\\EventLog\\LOGNAME",
        "Subkey1": "File",
        "Value1": "non-standard log location",
        "Subkey2": "Flag",
        "Value2": "1"
      },
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13,
        "label-font-size": 13
      }
    },
    {
      "id": "n27",
      "position": {
        "x": -6995.640046224957,
        "y": -2572.0915311231765
      },
      "caption": "Call API",
      "labels": [
        "CS RegSystemConfigValueUpdate",
        "WinEvent 4657 (requires SACL)"
      ],
      "properties": {
        "API": "NtSetValueKey ",
        "Key": "HKLM\\SYSTEM\\CurrentControlSet\\Services\\EventLog\\LOGNAME",
        "Subkey1": "File",
        "Value1": "standard log location",
        "Subkey2": "Flag",
        "Value2": "<deleted>"
      },
      "style": {
        "border-color": "#68bc00",
        "property-font-size": 13,
        "label-font-size": 13
      }
    },
    {
      "id": "n28",
      "position": {
        "x": -6764.640046224957,
        "y": -2572.0915311231765
      },
      "caption": "Call API",
      "labels": [],
      "properties": {
        "API": "NtDeleteFile",
        "File": "non-standard log location"
      },
      "style": {
        "border-color": "#808080",
        "property-font-size": 13,
        "label-font-size": 13
      }
    }
  ],
  "relationships": [
    {
      "id": "n1",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n23",
      "toId": "n24"
    },
    {
      "id": "n3",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n22",
      "toId": "n26"
    },
    {
      "id": "n4",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n26",
      "toId": "n23"
    },
    {
      "id": "n5",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n24",
      "toId": "n27"
    },
    {
      "id": "n6",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n27",
      "toId": "n25"
    },
    {
      "id": "n7",
      "type": "",
      "style": {},
      "properties": {},
      "fromId": "n25",
      "toId": "n28"
    }
  ]
}
```


## ddm_trr0018_ad_all.json

A single consolidated master holding all of a technique's procedures together (the '_all' style), all black.

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 2,
    "border-color": "#000000",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 16,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 1,
    "label-font-size": 16,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 5,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 16,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n0",
      "position": {
        "x": 209.41164585460146,
        "y": 312.59694033726106
      },
      "caption": "Enumerate SPNs",
      "labels": [],
      "properties": {
        "Host": "Any",
        "Option1": "Filter for SPNs in query",
        "Option2": "Get all users with SPN attribute then filter"
      },
      "style": {
        "border-color": "#68bc00"
      }
    },
    {
      "id": "n1",
      "position": {
        "x": 287.9330703833923,
        "y": 551.5656419911977
      },
      "caption": "Send ADWS request",
      "labels": [],
      "properties": {
        "Protocol": "TCP",
        "Port": "9389"
      },
      "style": {
        "border-color": "#68bc00"
      }
    },
    {
      "id": "n2",
      "position": {
        "x": 75,
        "y": 757.822255186173
      },
      "caption": "Send LDAP Search Request",
      "labels": [],
      "properties": {
        "Protocol": "LDAP or LDAPs",
        "Port": "389 or 636"
      },
      "style": {
        "border-color": "#68bc00"
      }
    },
    {
      "id": "n3",
      "position": {
        "x": 668.80608190224,
        "y": 757.822255186173
      },
      "caption": "Receive LDAP Search Request",
      "labels": [
        "CS ActiveDirectoryIncomingLdapSearchRequest",
        "DefATP LdapSearch"
      ],
      "properties": {},
      "style": {
        "border-color": "#0062b1"
      }
    },
    {
      "id": "n4",
      "position": {
        "x": 539.471644920577,
        "y": 551.5656419911977
      },
      "caption": "Receive ADWS request",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#0062b1"
      }
    },
    {
      "id": "n5",
      "position": {
        "x": 591.9116458546015,
        "y": 312.59694033726106
      },
      "caption": "Send AD Data",
      "labels": [],
      "properties": {
        "Host": "DC"
      },
      "style": {
        "border-color": "#0062b1"
      }
    },
    {
      "id": "n6",
      "position": {
        "x": 836.5664926656315,
        "y": 312.5969337300116
      },
      "caption": "Request Service Ticket",
      "labels": [],
      "properties": {
        "Host": "Any",
        "Protocol": "Kerberos",
        "Request": "TGS-REQ",
        "Needs": "Valid TGT",
        "Optional": "Downgrade encryption"
      },
      "style": {
        "border-color": "#68bc00"
      }
    },
    {
      "id": "n7",
      "position": {
        "x": 1081.2213394766618,
        "y": 312.59694033726106
      },
      "caption": "Send Service Ticket",
      "labels": [
        "CS ActiveDirectoryServiceAccessRequest",
        "Windows 4769"
      ],
      "properties": {
        "Host": "DC"
      },
      "style": {
        "border-color": "#0062b1"
      }
    },
    {
      "id": "n8",
      "position": {
        "x": 1303.281033098722,
        "y": 312.596940337261
      },
      "caption": "Crack Hash",
      "labels": [],
      "properties": {
        "Host": "Any"
      },
      "style": {
        "border-color": "#68bc00"
      }
    },
    {
      "id": "n9",
      "position": {
        "x": 954.0490025457719,
        "y": 168.00550614980182
      },
      "caption": "Intercept Service Ticket",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#68bc00"
      }
    },
    {
      "id": "n10",
      "position": {
        "x": 954.0490025457719,
        "y": 50
      },
      "caption": "Harvest Service Ticket",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#68bc00"
      }
    }
  ],
  "relationships": [
    {
      "id": "n0",
      "fromId": "n0",
      "toId": "n1",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n1",
      "fromId": "n0",
      "toId": "n2",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n2",
      "fromId": "n2",
      "toId": "n3",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n3",
      "fromId": "n1",
      "toId": "n4",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n4",
      "fromId": "n3",
      "toId": "n5",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n5",
      "fromId": "n5",
      "toId": "n6",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n6",
      "fromId": "n6",
      "toId": "n7",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n7",
      "fromId": "n7",
      "toId": "n8",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n8",
      "fromId": "n4",
      "toId": "n5",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n9",
      "fromId": "n7",
      "toId": "n6",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n10",
      "fromId": "n9",
      "toId": "n8",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n11",
      "fromId": "n10",
      "toId": "n8",
      "type": "",
      "properties": {},
      "style": {}
    }
  ]
}
```


## ddm_trr0020_azr_c.json

A cloud (Azure) DDM - fewer, control-plane operations with identity/API telemetry rather than OS internals. Uses a grey node for an operation that normally occurs but is skipped here.

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 4,
    "border-color": "#000000",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 16,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 2,
    "label-font-size": 14,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 5,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 16,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n0",
      "position": {
        "x": 75,
        "y": 227.20101756214115
      },
      "caption": "Create Service Principal",
      "labels": [
        "Entra AuditLog"
      ],
      "properties": {
        "RBAC": "Entra ID",
        "Permission": "microsoft.directory/servicePrincipals/create",
        "Operation": "Add service principal"
      },
      "style": {
        "border-color": "#808080",
        "label-position": "outside",
        "label-font-size": 14,
        "label-border-width": 2,
        "border-width": 3,
        "caption-font-size": 16,
        "node-color": "#cccccc"
      }
    },
    {
      "id": "n1",
      "position": {
        "x": 609.1249364223556,
        "y": 98.86661354056423
      },
      "caption": "Add Member to Role",
      "labels": [
        "Entra AuditLog"
      ],
      "properties": {
        "Operation1": "Add member to role*",
        "Operation2": "Add eligible member to role*",
        "RBAC": "Entra ID"
      },
      "style": {
        "border-color": "#68bc00",
        "border-width": 3,
        "label-position": "outside",
        "label-border-width": 2,
        "label-font-size": 14
      }
    },
    {
      "id": "n2",
      "position": {
        "x": 609.1249364223556,
        "y": 446.70060554148785
      },
      "caption": "Add Member to Role",
      "labels": [
        "Azure ActivityLog"
      ],
      "properties": {
        "Operation": "Microsoft.Authorization/roleAssignments/write",
        "RBAC": "Azure"
      },
      "style": {
        "border-color": "#68bc00",
        "border-width": 3,
        "label-position": "outside",
        "label-border-width": 2,
        "label-font-size": 14
      }
    },
    {
      "id": "n3",
      "position": {
        "x": 346.2813327552692,
        "y": 50
      },
      "caption": "Modify Custom Entra Role",
      "labels": [
        "Entra AuditLog"
      ],
      "properties": {
        "Operation": "Update role definition",
        "RBAC": "Entra ID"
      },
      "style": {
        "border-color": "#68bc00",
        "border-width": 3,
        "label-position": "outside",
        "label-border-width": 2,
        "label-font-size": 14
      }
    },
    {
      "id": "n4",
      "position": {
        "x": 346.2813327552692,
        "y": 446.70060554148785
      },
      "caption": "Create or Modify Custom Azure Role",
      "labels": [
        "Azure ActivityLog"
      ],
      "properties": {
        "Operation": "Microsoft.Authorization/roleDefinitions/write",
        "RBAC": "Azure"
      },
      "style": {
        "border-color": "#68bc00",
        "border-width": 3,
        "label-position": "outside",
        "label-border-width": 2,
        "label-font-size": 14
      }
    },
    {
      "id": "n5",
      "position": {
        "x": 346.2813327552692,
        "y": 180.5926359905584
      },
      "caption": "Create Custom Entra Role",
      "labels": [
        "Entra AuditLog"
      ],
      "properties": {
        "Operation": "Add role definition"
      },
      "style": {
        "border-color": "#68bc00",
        "border-width": 3
      }
    }
  ],
  "relationships": [
    {
      "id": "n0",
      "fromId": "n4",
      "toId": "n2",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n1",
      "fromId": "n3",
      "toId": "n1",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n2",
      "fromId": "n0",
      "toId": "n3",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n3",
      "fromId": "n0",
      "toId": "n4",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n4",
      "fromId": "n0",
      "toId": "n5",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n5",
      "fromId": "n5",
      "toId": "n1",
      "type": "",
      "properties": {},
      "style": {}
    }
  ]
}
```


## trr0013_a.json

A larger, more complex DDM (21 operations) that uses grey nodes for skipped operations and grey arrows for secondary/optional flow.

```json
{
  "style": {
    "font-family": "sans-serif",
    "background-color": "#ffffff",
    "background-image": "",
    "background-size": "100%",
    "node-color": "#ffffff",
    "border-width": 2,
    "border-color": "#000000",
    "radius": 50,
    "node-padding": 5,
    "node-margin": 2,
    "outside-position": "auto",
    "node-icon-image": "",
    "node-background-image": "",
    "icon-position": "inside",
    "icon-size": 64,
    "caption-position": "inside",
    "caption-max-width": 200,
    "caption-color": "#000000",
    "caption-font-size": 16,
    "caption-font-weight": "normal",
    "label-position": "outside",
    "label-display": "pill",
    "label-color": "#000000",
    "label-background-color": "#ffffff",
    "label-border-color": "#000000",
    "label-border-width": 1,
    "label-font-size": 16,
    "label-padding": 5,
    "label-margin": 4,
    "directionality": "directed",
    "detail-position": "inline",
    "detail-orientation": "parallel",
    "arrow-width": 5,
    "arrow-color": "#000000",
    "margin-start": 5,
    "margin-end": 5,
    "margin-peer": 20,
    "attachment-start": "normal",
    "attachment-end": "normal",
    "relationship-icon-image": "",
    "type-color": "#000000",
    "type-background-color": "#ffffff",
    "type-border-color": "#000000",
    "type-border-width": 0,
    "type-font-size": 16,
    "type-padding": 5,
    "property-position": "outside",
    "property-alignment": "colon",
    "property-color": "#000000",
    "property-font-size": 16,
    "property-font-weight": "normal"
  },
  "nodes": [
    {
      "id": "n0",
      "position": {
        "x": 1059.863862859057,
        "y": 245.61052169284716
      },
      "caption": "Send Network Request",
      "labels": [],
      "properties": {
        "Host": "Any",
        "User": "Any",
        "Kerberos Type": "TGS-REQ"
      },
      "style": {
        "node-color": "#ffffff",
        "border-color": "#68bc00",
        "property-font-size": 12,
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2
      }
    },
    {
      "id": "n1",
      "position": {
        "x": 1322.8385595702298,
        "y": 145.6331923870195
      },
      "caption": "Receive Network Request",
      "labels": [
        "Event 4769",
        "CS ActiveDirectoryServiceAccessRequest"
      ],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "TGS-REQ"
      },
      "style": {
        "border-color": "#0062b1",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2,
        "property-font-size": 12
      }
    },
    {
      "id": "n2",
      "position": {
        "x": 1059.863862859057,
        "y": 480.61317858439565
      },
      "caption": "Send TCP Packet",
      "labels": [
        "IDS Rule"
      ],
      "properties": {
        " Port": "Any"
      },
      "style": {
        "border-color": "#68bc00",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2,
        "property-font-size": 12
      }
    },
    {
      "id": "n3",
      "position": {
        "x": 1322.8385595702298,
        "y": 480.61317858439565
      },
      "caption": "Receive TCP Packet",
      "labels": [],
      "properties": {
        " Port": "88",
        "Host": "DC"
      },
      "style": {
        "border-color": "#0062b1",
        "caption-font-size": 14,
        "property-font-size": 12,
        "label-font-size": 12
      }
    },
    {
      "id": "n4",
      "position": {
        "x": 1567.16809701075,
        "y": 145.6331923870195
      },
      "caption": "Send Network Response",
      "labels": [],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "TGS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#0062b1",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n5",
      "position": {
        "x": 1567.16809701075,
        "y": 481.69344975505305
      },
      "caption": "Send TCP Packet",
      "labels": [],
      "properties": {
        "Host": "DC",
        " Port": "88"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#0062b1",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n6",
      "position": {
        "x": 1811.5202906002667,
        "y": 481.14227253222464
      },
      "caption": "Receive TCP Packet",
      "labels": [],
      "properties": {
        " Port": "Any"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n7",
      "position": {
        "x": 1811.5202906002667,
        "y": 215.8799887928358
      },
      "caption": "Receive Network Request",
      "labels": [],
      "properties": {
        "Kerberos Type": "TGS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#68bc00",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n8",
      "position": {
        "x": 75,
        "y": 236.79007894270794
      },
      "caption": "Send Network Request",
      "labels": [],
      "properties": {
        "Host": "Any",
        "User": "Any",
        "Kerberos Type": "AS-REQ"
      },
      "style": {
        "node-color": "#ffffff",
        "border-color": "#cccccc",
        "property-font-size": 12,
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2
      }
    },
    {
      "id": "n9",
      "position": {
        "x": 332.77397524247755,
        "y": 181.11811219932525
      },
      "caption": "Receive Network Request",
      "labels": [
        "Event 4768",
        "CS ActiveDirectoryAuthentication"
      ],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "AS-REQ"
      },
      "style": {
        "border-color": "#666666",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2,
        "property-font-size": 12
      }
    },
    {
      "id": "n10",
      "position": {
        "x": 75,
        "y": 480.61317858439565
      },
      "caption": "Send TCP Packet",
      "labels": [
        "IDS Rule"
      ],
      "properties": {
        " Port": "Any"
      },
      "style": {
        "border-color": "#cccccc",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2,
        "property-font-size": 12
      }
    },
    {
      "id": "n11",
      "position": {
        "x": 332.77397524247755,
        "y": 480.61317858439565
      },
      "caption": "Receive TCP Packet",
      "labels": [],
      "properties": {
        " Port": "88",
        "Host": "DC"
      },
      "style": {
        "border-color": "#666666",
        "caption-font-size": 14,
        "property-font-size": 12,
        "label-font-size": 12
      }
    },
    {
      "id": "n12",
      "position": {
        "x": 2074.485037391658,
        "y": 218.16757600928455
      },
      "caption": "Send Network Request",
      "labels": [],
      "properties": {
        "Host": "Any",
        "User": "Any",
        "Kerberos Type": "AP-REQ"
      },
      "style": {
        "node-color": "#ffffff",
        "border-color": "#68bc00",
        "property-font-size": 12,
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2
      }
    },
    {
      "id": "n13",
      "position": {
        "x": 2337.4696840226134,
        "y": 181.11811219932525
      },
      "caption": "Receive Network Request",
      "labels": [
        "Event 4624"
      ],
      "properties": {
        "Host": "Desired Service",
        "Kerberos Type": "AP-REQ"
      },
      "style": {
        "border-color": "#7b64ff",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2,
        "property-font-size": 12
      }
    },
    {
      "id": "n14",
      "position": {
        "x": 2074.49498731144,
        "y": 481.14227253222464
      },
      "caption": "Send TCP Packet",
      "labels": [
        "IDS Rule"
      ],
      "properties": {
        " Port": "Any"
      },
      "style": {
        "border-color": "#68bc00",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2,
        "property-font-size": 12
      }
    },
    {
      "id": "n15",
      "position": {
        "x": 2337.4696840226134,
        "y": 481.14227253222464
      },
      "caption": "Receive TCP Packet",
      "labels": [],
      "properties": {
        " Port": "Varies",
        "Host": "Desired Service"
      },
      "style": {
        "border-color": "#7b64ff",
        "caption-font-size": 14,
        "property-font-size": 12,
        "label-font-size": 12
      }
    },
    {
      "id": "n16",
      "position": {
        "x": 832.7606307373092,
        "y": 50
      },
      "caption": "Forge TGT",
      "labels": [],
      "properties": {},
      "style": {
        "border-color": "#68bc00",
        "caption-font-size": 14,
        "label-position": "outside",
        "label-font-size": 12,
        "label-border-width": 2,
        "property-font-size": 12
      }
    },
    {
      "id": "n17",
      "position": {
        "x": 562.220433351948,
        "y": 181.11811219932525
      },
      "caption": "Send Network Response",
      "labels": [],
      "properties": {
        "Host": "DC",
        "Kerberos Type": "AS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#666666",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n18",
      "position": {
        "x": 562.220433351948,
        "y": 481.69344975505305
      },
      "caption": "Send TCP Packet",
      "labels": [],
      "properties": {
        "Host": "DC",
        " Port": "88"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#666666",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n19",
      "position": {
        "x": 832.7606307373092,
        "y": 481.69344975505305
      },
      "caption": "Receive TCP Packet",
      "labels": [],
      "properties": {
        " Port": "Any"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#999999",
        "label-font-size": 12,
        "property-font-size": 12
      }
    },
    {
      "id": "n20",
      "position": {
        "x": 832.7606307373092,
        "y": 245.61052169284716
      },
      "caption": "Receive Network Response",
      "labels": [],
      "properties": {
        "Kerberos Type": "AS-REP"
      },
      "style": {
        "caption-font-size": 14,
        "border-color": "#999999",
        "label-font-size": 12,
        "property-font-size": 12
      }
    }
  ],
  "relationships": [
    {
      "id": "n0",
      "fromId": "n2",
      "toId": "n3",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n1",
      "fromId": "n0",
      "toId": "n2",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n2",
      "fromId": "n3",
      "toId": "n1",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n3",
      "fromId": "n1",
      "toId": "n4",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n4",
      "fromId": "n4",
      "toId": "n5",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n5",
      "fromId": "n5",
      "toId": "n6",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n6",
      "fromId": "n6",
      "toId": "n7",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n7",
      "fromId": "n10",
      "toId": "n11",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n8",
      "fromId": "n8",
      "toId": "n10",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n9",
      "fromId": "n11",
      "toId": "n9",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n10",
      "fromId": "n14",
      "toId": "n15",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n11",
      "fromId": "n12",
      "toId": "n14",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n12",
      "fromId": "n15",
      "toId": "n13",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n13",
      "fromId": "n7",
      "toId": "n12",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n14",
      "fromId": "n16",
      "toId": "n0",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n15",
      "fromId": "n17",
      "toId": "n18",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n16",
      "fromId": "n9",
      "toId": "n17",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n17",
      "fromId": "n19",
      "toId": "n20",
      "type": "",
      "properties": {},
      "style": {
        "property-font-size": 12
      }
    },
    {
      "id": "n18",
      "fromId": "n18",
      "toId": "n19",
      "type": "",
      "properties": {},
      "style": {}
    },
    {
      "id": "n19",
      "fromId": "n20",
      "toId": "n0",
      "type": "",
      "properties": {},
      "style": {}
    }
  ]
}
```

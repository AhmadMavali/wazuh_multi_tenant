# wazuh_multi_tenant
setup wazuh to serve multiple organizations on a single instance.




Tenants in the Wazuh dashboard are spaces for saving index patterns, visualizations, dashboards, and other objects. Tenants are useful for safely sharing your work with other users. You can control which roles have access to a tenant and whether those roles have read or write access. By default, all the Wazuh dashboard users have access to two independent tenants:

Global: This tenant is shared between every Wazuh dashboard user.

Private: This tenant is exclusive to each user and can’t be shared. You can’t use it to access routes or index patterns made by the user’s global tenant.

An example 
Two different organizations need to see and manage only their agents
For this, you must first create 2 groups and add the agents of each organization to their own group.



![Capture](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/4af25bb1-fa2a-4a52-b04f-ed51fe911827)



In the group configuration, you must use the LABEL to distinguish the groups


The first group is related to the first organization
The following tag should be used


![a](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/ac219533-3b1a-4bfc-b8ae-1e173c2d42e7)



For the second organization, you should use different labels in your group configuration so that these two groups are separate from each other


![b](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/257e8e0c-c8da-49b4-96ef-fb772faf40c7)



Now we need to define roles for both organizations in the OpenSearch security section (Not Wazuh section)

![roleopen](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/9ed0c5ea-a454-44b6-b6a9-e318fbbcaa12)


in the document level security section 


{
    "bool": {
      "must": [],
      "filter": [
        {
          "match_all": {}
        },
        {
          "match_phrase": {
            "agent.labels.org-1": "org-1"
          }
        }        
      ],
      "should": [],
      "must_not": []
    }
}


then  create another role for the second organization The settings are the same as before, except that in the section document level security
 
{
    "bool": {
      "must": [],
      "filter": [
        {
          "match_all": {}
        },
        {
          "match_phrase": {
            "agent.labels.org-2": "org-2"
          }
        }        
      ],
      "should": [],
      "must_not": []
    }
}


In creating each role, a user must be mapped, and for both organizations, we mapped the user org-1 and org-2 (internal user creation)


in the /usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml file we should have set True in line run_as: true


![ss](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/dc2505e6-a078-47a5-a3ad-ee4d2781f71b)


in wazuh management --> security section select policies create two policies for our Organizations

first organization

![www](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/5b66ad26-1aaa-451b-a4e2-cac2f7641e23)



second organization

![fffff](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/f63f3563-ae60-49de-bd56-c1e3acd4c9ac)




then select  role on the same page 




On the same page, we go to the Roles section

We create the role for the policy we made.



first role
![role1](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/04316b51-3129-4ebe-8c44-9f56c3441e5f)


second role
![role2](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/17c3446f-d4c1-479a-a9c0-fc47b8b60ffd)




On the same page, we go to the roles mapping section



first role mapping 
![ppppppp](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/bfe149ce-32da-4ae5-956c-f034b2195f96)




second role mapping 

![ppppppppppppppppp](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/6b6e8a08-cf6a-4e3a-bc0c-017f8bf8034b)


finally in the/etc/wazuh-dashboard/opensearch_dashboards.yml file we change/add  lines like this 


![Ca](https://github.com/AhmadMavali/wazuh_multi_tenant/assets/102754122/9ca97733-688a-4512-a32e-e7519cb2cadf)





thanks :)











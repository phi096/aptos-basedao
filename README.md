
# BaseDAO

_**Collectively building the future ecosystem on Aptos**_

BaseDAO simplifies the creation and management of Decentralized Autonomous Organisations (DAOs) on the Aptos blockchain.

With just one click, anyone can establish a DAO for their community, choosing a governance model that aligns closest with their group’s unique needs and preferences.

While DAOs originated within the crypto and blockchain space, their applications extend to a variety of real-world domains, such as life sciences with LabDAO and humanitarian projects through Impact DAOs.

As technology and the world evolves, DAOs have become an essential tool for decentralised governance that can support a wide range of purposes — from investment groups to project collaborations, and even initiatives aimed at global change.

## DAO Models for Different Community Needs

BaseDAO offers various governance models to suit different types of communities:

**Standard DAO**: This model supports collective decision-making, allowing members that hold the DAO’s governance tokens to participate in proposing, voting on, and executing ideas together.

**Guild DAO**: Designed for specialised and focused groups, the Guild DAO model incorporates a centralized leadership structure without the need for governance tokens, making it a good fit for small to medium communities.

**Hybrid DAO**: For communities that want a blend of democratic governance and role-based structures, the Hybrid DAO model offers flexibility with governance tokens and member roles together that influences a member’s vote and participation.

With BaseDAO, we want to abstract away the technical complexities, making it extremely easy for anyone to set up and manage a DAO, regardless of their technical expertise — so they can focus on what truly matters: building their community.

## Governance Process and Proposal Types

BaseDAO allows each DAO to tailor its governance mechanisms to fit its unique structure.

A core element of this customization is Proposal Types, which control aspects such as the duration of a proposal, the required percentage of votes from the total governance token supply for execution, and the minimum amount needed to either vote or create a proposal of the given type.

Upon initialisation, each DAO is currently provided with a default Proposal Type. However, looking ahead, we plan to introduce two additional default Proposal Types to cater to various scenarios.

For instance, DAOs could utilize quick proposals to gauge community sentiment on specific issues or longer proposals requiring a higher success threshold for serious topics that warrant extended discussion.

Proposal Types themselves can be added, modified, or removed through governance actions. In addition, in Guild DAOs and Hybrid DAOs, members holding leader roles (or those with a vote weight equal to or greater than the leader's minimum threshold) will be able to customise proposal types as well.

During a proposal's voting period (based on its corresponding proposal type), qualified users will be able to vote YAY, NAY, or PASS on the proposal. For Standard DAOs, qualified members would need to hold governance tokens equal to or above the min_amount_to_vote property in the Proposal Type. In contrast, for Guild DAOs, qualified members would required an assigned a role with a weightage equal to or above min_amount_to_vote.

On the other hand, for Hybrid DAOs, user roles serve as a multiplier for their governance token balance. As such, even users without roles but holding sufficient governance tokens above the threshold would be able to vote. Essentially, these roles magnify or diminish users' voting weight on a customisable and flexible basis.

Users will also be able to change their votes at any time before the proposal ends; their previous vote count will be removed, and a new vote count reflecting their current governance token balance or vote weight will be registered for the Standard DAO and Guild DAO respectively.

After a proposal's voting period ends, if it accumulates sufficient YAY votes to surpass the success threshold, any member can execute it. For example, suppose a Fungible Asset transfer proposal is executed successfully. In that case, it will trigger a transfer from the DAO module to the intended recipient with the specified amount in the proposal.

This execution mechanism applies to all proposal types except the standard proposal, which is intended for general discussions and polls rather than on-chain actions.

Through these flexible and customisable mechanisms, various groups and organisations will be able to utilise the DAO models on BaseDAO to suit their needs and purposes.

## Tech Overview and Considerations

We follow the Aptos Object Model approach, storing Proposals on user accounts rather than on the DAO contract to decentralise data storage, enhance scalability, and optimise gas costs.

The DAO contract maintains a ProposalRegistry Struct that maps proposal IDs to their respective creators. Proposal IDs are unique and sequentially assigned, ensuring that no two proposals share the same ID, regardless of their creator.

## Smart Contract Entrypoints

The BaseDAO DAO module entrypoints includes eleven public entrypoints that are common to each of the DAO models (standard, guild, and hybrid):

**General DAO Entrypoints**

1.  **init_dao**: Initialises a new DAO. This can only be called once.
    
    -   **Input**: DAO name, description, image, and governance token metadata
        
    -   **Output**: Initialises a DAO on the given module
        
2.  **deposit_fa_to_dao**: Deposits a specified fungible asset to the DAO module
    
    -   **Input**: Depositor signer, amount, and fungible asset token metadata
        
    -   **Output**: Transfers tokens from the depositor to the DAO
        
3.  **deposit_coin_to_dao**: Deposits a specified coin to the DAO module
    
    -   **Input**: Depositor signer and amount,
        
    -   **Output**: Transfers coins from the depositor to the DAO
        
4.  **create_fa_transfer_proposal**: Creates a proposal to transfer fungible assets
    
    -   **Input**: Proposal title, description, proposal type, and FA transfer parameters
        
    -   **Output**: Creates new FA transfer proposal
        
5.  **create_coin_transfer_proposal**: Creates a proposal to transfer coins
    
    -   **Input**: Proposal title, description, proposal type, and coin transfer parameters
        
    -   **Output**: Creates new coin transfer proposal
        
6.  **create_proposal_update_proposal**: Creates a proposal to update or remove proposal types
    
    -   **Input**: Proposal title, description, proposal type, and new proposal type parameters
        
    -   **Output**: Creates new proposal update proposal
        
7.  **create_dao_update_proposal**: Creates a proposal to update the DAO info
    
    -   **Input**: Proposal title, description, proposal type, and new DAO info (name, description, image)
        
    -   **Output**: Creates new DAO update proposal
        
8.  **create_standard_proposal**: Creates a standard miscellaneous proposal to get members voting preferences on an issue
    
    -   **Input**: Proposal title, description, proposal type
        
    -   **Output**: Creates new standard proposal
        
9.  **vote_for_proposal**: Allows a user to vote for a given proposal if he has sufficient governance tokens above the threshold specified in the proposal type
    
    -   **Input**: Voter, proposal ID and vote type
        
    -   **Output**: Votes on a proposal
        
10.  **execute_proposal**: Allows a user to executes a proposal if it has garnered sufficient votes. If insufficient votes were gathered, the proposal result would be marked as "FAIL"
    
    -   **Input**: Proposal ID
        
    -   **Output**: Executes a proposal
        
11.  **execute_coin_transfer_proposal**: Allows a user to executes a coin transfer proposal if it has garnered sufficient votes. If insufficient votes were gathered, the proposal result would be marked as "FAIL"
    
    -   **Input**: Proposal ID and a given CoinType that should match the proposal to be executed
        
    -   **Output**: Executes a coin transfer proposal
        

**Additional Guild/Hybrid DAO Entrypoints**

**Leader Role Entrypoints:**

12.  **update_executive_vote_weight**: Allows leaders with sufficient vote weight above or equal to min_leader_vote_weight to update the min_executive_vote_weight for member access to executive-level entrypoints
    
    -   **Input**: Leader signer and new min_executive_vote_weight
        
    -   **Output**: Updates the min_executive_vote_weight
        
13.  **update_leader_vote_weight**: Allows leaders with sufficient vote weight above or equal to min_leader_vote_weight to update the min_leader_vote_weight for member access to leader-level entrypoints
    
    -   **Input**: Leader signer and new min_leader_vote_weight
        
    -   **Output**: Updates the min_leader_vote_weight
        
14.  **add_or_update_proposal_types**: Allows leaders to add or update proposal types
    
    -   **Input**: Leader signer and new proposal type parameters
        
    -   **Output**: Creates or updates a new proposal type
        
15.  **remove_proposal_types**: Allows leaders to remove proposal types
    
    -   **Input**: Leader signer and proposal type name
        
    -   **Output**: Removes a new proposal type
        

**Executive Role Entrypoints:**

16.  **add_or_update_role**: Allows leaders or executives to add or update roles
    
    -   **Input**: Executive signer, role, and role weight
        
    -   **Output**: Creates or updates a role
        
17.  **remove_role**: Allows leaders or executives to remove roles
    
    -   **Input**: Executive signer and role name
        
    -   **Output**: Removes a role
        
18.  **add_or_update_member**: Allows leaders or executives to add or update members
    
    -   **Input**: Executive signer, member address, and member role
        
    -   **Output**: Creates or updates the given member with the given member role
        
19.  **remove_member**: Allows leaders or executives to remove members
    
    -   **Input**: Executive signer and member address
        
    -   **Output**: Removes the given member
        

## Code Coverage

BaseDAO has comprehensive test coverage, with 100% of the codebase thoroughly tested. This includes a full range of scenarios that ensure the platform's reliability and robustness.

The following section provides a breakdown of the tests that validate each function and feature, affirming that BaseDAO performs as expected under all intended use cases.

## Future Plans

Looking ahead, here are some plans to expand the features and capabilities of BaseDAO in Phase 2.

### Planned Features:

-   **Expand DAO Governance Models**: As more communities adopt decentralized governance, BaseDAO aims to expand its offering with additional governance models tailored for specific use cases. For example, a “Quadratic Voting DAO” could cater to communities interested in nuanced decision-making, while a “DAO-as-a-Service” model could support organizations needing turnkey solutions for various governance structures.
    
-   **Launch DAO Toolkits for Community Management**: With a growing user base, we would also be able to develop and launch a suite of community management tools, including event scheduling, treasury management, and collaboration features. These toolkits will provide a comprehensive set of resources for communities to thrive and grow further.
    
-   **Onboarding and Education Programs**: As DAOs continue to grow in popularity, BaseDAO will prioritize education and onboarding resources to help new users understand the value of decentralized governance. These programs could include workshops, tutorials, and online courses focused on DAO creation, governance best practices, and community engagement strategies.
    

## Conclusion

BaseDAO is committed to lowering the barriers to decentralized governance, enabling anyone to build and manage a DAO with ease on Aptos.

By expanding our governance models, enhancing integrations, and offering robust community tools, BaseDAO seeks to be the go-to platform for DAO creation and management.

As the world continues to embrace decentralized technologies, BaseDAO will be at the forefront, empowering communities to build, collaborate, and drive meaningful change.

## Credits and Support

BaseDAO is designed and built by 0xBlockBard, a solo indie maker passionate about building innovative products in the web3 space.

With over 10 years of experience, my work spans full-stack and smart contract development. I’m also familiar with Solidity, Rust, LIGO, and most recently, Aptos Move.

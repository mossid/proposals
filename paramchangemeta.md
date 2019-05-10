# Parameter change proposal meta-proposal

## Abstract

This paragraph is to help understanding the proposal, and does not have any effect.

The purpose of this proposal is the followings:

- To minimise the confusion of the participants from unexpected and unprepared parameter change

Most of the parameter that is used by the Cosmos hub is directly related with the proof of
stake algorithm, which ensures the safety of the blockchain, and the rest are also related
with the operation of the blockchain, such as governance or gas fee. Changing parameter
requires the users to update their mental model of the blockchain, and the cost of it
increases proportional to the growth of the number of network users and the value. However
the cost of governance vote does not increase, because the set of validators, who have
the obligation to vote for every proposal, is limited.

For example, parameter `auth.KeySigVerifyCostEd25519` defines the gas cost for proving
signature of a transaction. In the current form of parameter change proposal where it
is not restricted at all, if there is a reason behind the proposal, voters will vote for
`YES` since there is no special reason to vote `NO`, so we can expect that there will
be frequent parameter change. However this means that the wallet developers and the users
have to face the frequent change of the transaction fee.

The "weak" restriction defines which parmeter change proposal is **allowed**. The voters
are recommended to vote `No` or `NoWithVeto` on the proposal which does not satisfy the
weak restrictions.

- To prevent enacting of proposals which can halt the state machine

Parameter change proposals are unrestricted on which parameter and value that they can change,
so some of the proposal can halt the blockchain when enacted. This includes state machine halt
due to a systematically wrong parameter, validator disincentivization due to a wrong staking
parameter, governance process stuck due to a wrong governance parameter. In these scenarios,
the blockchain cannot recover itself, only an offchain hard fork can solve it, which is costly.
Invalid parameters can be clarified with analysing the code. Parameter change should not passed
when it contains such parameters.

The "strong" restriction defines which parameter change is **denied**. The voters are
recommended to vote `NoWithVeto` on the proposal which does not satisfy the strong restrictions.

- To require more attention on unexpected parameter change

If a proposer wants to propose a parameter change proposal which does not satisfy a restriction,
they have to first propose a `TextProposal` which explicitly amend an article which defines
the restriction. Unlike parameter change proposal, this text proposal, amends the whole
restriction, not a single parameter value, so the voters will have to consider the general
case that this restriction will be applied.

If an attacker(or a mistaker) tries to propose a parameter change proposal which does not satisfy
the restrictions and harms the chain, even if they have a reason for the proposal, they first
have to prove that the restriction is *fundamentally* invalid. Also, now it takes twice more time
to fully change the parameter, the voters have more time to analyse the proposal and find
potential vulnerability.

## Articles

### A-Articles

A1. Definitions

a) "Text proposal" is defined as `gov.TextProposal`. Text proposal is for making onchain
agreement between the voters. When passed the governance process, it does not modify the
state except which is part of the governance process.

b) "Parameter change proposal" is defined as `params.ParameterChangeProposal`. When passed the
governance process, it automatically changes one or more than one parameters depending on the
values specified in the proposal, without human intervention or chain pause.

A2. Effects

a) This proposal is submitted as a text proposal, and takes effect immediately after it passed
the governance process.

b) This proposal takes effect on all parameter change proposals which enters voting period
after this proposal is passed. The voters have to consider the latest amended version of this
proposal as of the proposal entered the voting period, and code that the chain is running on,
before voting on any parameter change proposals.

c) A-Articles cannot be enacted, amended, or repealed after this proposal takes effect. If any
of the A-articles needs to be amended, the voters have to propose a new text proposal which
explicitly replacing this proposal. In this case, this proposal does not effect on any parameter
change proposal that has entered the voting period after it is replaced, but holds exclusive
effect on the proposals that it is already effecting. In this case, the references to this
proposals on other proposals are treated as referring the new proposal.

e) The definitions should be interpreted on the latest version of the Cosmos-SDK as of the time
this proposal is being written on, which is v0.34.4. If the chain has upgraded to a version
which does not contains the code corresponding to the definitions of this proposal, this proposal
does not effect on any paramter change proposal that has entered the voting period after the
chain has upgraded.

f) W-Articles and S-Articles can be enacted if a text proposal explicitly enacting new
W-Articles and S-Articles passed the governance process.

g) W-Articles and S-Articles in this proposal can be amended or repealed if a text proposal
explicitly amending or repealing the articles passed the governance process.

A3. Weak Restrictions

a) Weak restrictions defines the accepted range of parameter change to make the chain consistently
grow and achieve its purpose and plan successfully. The weak restrictions have to be enacted
in way that the participants of the chain can expect the future direction of the chain.

b) Weak restrictions have to define the process of calculating the accepted range. The process
should be able to be performed by the average voters easily.

c) The voters should vote `No` or `NoWithVeto` when a parameter change proposal does not satisfy
any weak restrictions. However, in the exceptional case where the parameter change proposal is
urgent so it cannot wait for the amendment on this proposal, and it is clear that the chain's
value will be damaged if the proposal is not passed, the voters can ignore this paragraph.

d) Weak restrictions are defined by W-Articles

A4. Strong Restrictions

a) Strong restrictions defines the accepted range of parameter change to make the chain not halt and
not fail on its desired functionality. The strong restrictions have to be enacted in way that the
participants of the chain can expect that the chain will halt if a parameter not satisfying any
strong restriction is set.

b) Strong restrictions have to define the constant set of accepted parameter. The set is
pure mathematical expression, and does not depend on the state, neither onchain or offchain.
However, it can use the variables which does not change during the normal execution of the
chain but only changes on software upgrade.

c) The voters should vote `NoWithVeto` when a parameter change proposal does not satisfy any
strong restriction.

d) Strong restrictions are defined by S-Articles

### W-Articles

W1. `staking.MaxValidators`

a) `staking.MaxValidators` should be `100*(1.13)^y` with `13%` error margin, where `y` is
the number of years passed from the genesis block to the point of time where the proposal
entered the voting period, rounded off.

### S-Articles

S1. `staking.MaxValidators`

a) `staking.MaxValidators` should be equal or over `100`, and equal or under `300`.

# Solidity DevSecOps Standard.

DevSecOps standard for teams wishing to build secure software for EVM-based blockchain networks from the ground up and post deployment.
A more detailed guide is in development. This checklist itself is still being improved upon.

_Based on work by [ConsenSys Diligence](https://consensys.github.io/smart-contract-best-practices/), [the Secureum](https://secureum.xyz/) and inspired by the [solcurity standard](https://github.com/Rari-Capital/solcurity)._
_Additionally, I'd like to thank my colleague and mentor [Dominik Muhs](https://twitter.com/lethalspoons) for his feedback in making this guide._

### Philosophy

The concept behind this repo and the suggestions herein are to "shift left" your security in the software development life cycle as much as possible to spot edge cases and major bugs early. Additionally, they're meant to be an attempt at ensuring a high security standard throughout your project's existence. This has the effect of saving you from major rewrites of your code base and saving the time and money it would've taken to fix those issues later on. Of course, not getting exploited for millions of dollars is a win too.

### Human Elements/Mindset

- Educate developers on security best practices i.e [ConsenSys](https://consensys.github.io/smart-contract-best-practices/) and [Trail of Bits](https://github.com/crytic/building-secure-contracts)
- Have atleast one security engineer monitoring and reviewing commits for security pitfalls and potential vulnerabilities whether internally or contracted. If you absolutely cannot afford one then atleast have a security oriented developer on the team reviewing git commits with the [Solcurity Standard](https://github.com/Rari-Capital/solcurity).
- Code should be written according to the following principles and standards starting with [Saltzer and Shroeder's 10 secure design principles](https://github.com/morphean-sec/secure-smart-contract-design-principles) as applied to smart contracts.

### Code

- Find access control issues or other problems in your code with [Solgrep](https://github.com/tintinweb/solgrep) - A scriptable semantic grep utility for solidity
- Your code must be well tested(unit and integration) with ideally 100% code coverage. **Important**: Your README should give clear instructions for running the test suite. If any dependencies are not packaged with your code (e.g. Truffle), list them and their exact versions.
- Adhere to a standard programming style to improve readability and auditability i.e the official [solidity style](https://docs.soliditylang.org/en/v0.8.11/style-guide.html). 
- Use well tested libraries like [OpenZeppelin libraries](https://docs.openzeppelin.com/contracts/4.x/) to implement functions i.e "Don't roll your own cryptography" or "Don't reinvent the wheel." to minimize bugs and mistakes that may lead to disasters.
- Keep it clean i.e
1. Run a linter on your code. Fix any errors or warnings unless you have a good reason not to. For Solidity, we like Ethlint.
2. If the compiler outputs any warnings, address them.
3. Remove any comments that indicate unfinished work (ie. TODO or FIXME). (This is assuming it’s your final audit before deploying to mainnet. If not, exercise your judgement about what makes sense to leave in.)
4. Remove any code that has been commented out.
5. Remove any code you don’t need.
- Write [Scribble](https://consensys.net/diligence/scribble/) properties for robust and incremental fuzzing with MythX(Harvey) or Echidna upon commits. An introduction to fuzzing Ethereum smart contracts with Echidna from ToB itself can be found [here](https://github.com/crytic/building-secure-contracts/tree/master/program-analysis/echidna).
- Ensure your code has proper audit logs and event emissions set up. Effective alerting could mean the difference between being rekt and staying afloat.
- Consider importing and using the [Pausable library](https://docs.openzeppelin.com/contracts/4.x/api/security#Pausable) from OpenZeppelin as a multi-sig function(or centralized onlyOwner) in order to freeze the contract in case of an exploit.

### CI/CD Pipeline

- Integrate security tools into your CI. A few examples are [Slither](https://github.com/crytic/slither-action) and [Echidna](https://github.com/crytic/echidna-action) GitHub Actions, [Mythril](https://github.com/ConsenSys/mythril) and the paid SaaS security tools [MythX](https://mythx.io/) and [Diligence Fuzzing](https://consensys.net/diligence/fuzzing/). If it seems like a lot of work to you, use the integrated Foundry and DappTools security tools. However, it is strongly suggested to write your property tests with [Scribble](https://consensys.net/diligence/scribble/). A specification language from ConsenSys Diligence that takes your fuzzing and testing to a whole new level.
1. MythX recommended usage:
- Quick scan every commit.
- Standard scan at development milestones.
- Deep scan prior to audits/releases.

Fuzzing, static analysis and even symbolic execution should be done as often as possible to reduce security debt upon audits.

### Post deployment to mainnet

- Acquire insurance in case of exploits. An example of a project offering exploit insurance is [Fides](https://confidencesystem.webflow.io/).
- Set up a bug bounty programme for responsible disclosure via platforms like Fides and [Immunefi](https://immunefi.com/)
- Make sure your development/security team is easily accessible in case a security researcher wishes to alert you to a problem in your code. Establish points of contact i.e e-mail addresses, Telegram/Twitter usernames(with open DMs) etc in your project's Github README, website and via [Blockchain Security Contacts](https://github.com/crytic/blockchain-security-contacts)
- The security of the privileged wallets should be of the utmost importance and each of the holders(if using hardware wallets) MUST read and follow these [best practices](https://blog.trailofbits.com/2018/11/27/10-rules-for-the-secure-use-of-cryptocurrency-hardware-wallets/) without fail.
- Additionally, make your functions multi-sig where possible to lessen impact should an attacker manage to gain control of one.
- Ensure you have an incident response plan and assume that your smart contracts can and will be compromised. The code might be free of obvious bugs but an attacker might take over the contract owner's keys.

### Security alerts

Time is of the essence if your protocol is ever the victim of an exploit and so a proper alert system is crucial to your continued security.
- Set up alerts to certain Forta agents with OpenZeppelin's Defender Sentinel

### Off-chain Components

Off chain components of your decentralized(or not so decentralized) application are just as important as on-chain smart contracts even in web2. Ensuring the security of your frontend and backend is critical.

- Employ Security Engineers in testing common web vulnerabilities using the [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/) although this is far from a panacea. Assuming you have a good security professional on your team they will know what to do otherwise it's best to employ blockchain security firms whose expertise extends across the stack e.g [ConsenSys Diligence](https://diligence.consensys.net/), [Trail of Bits](https://www.trailofbits.com/), [Sigma Prime](https://sigmaprime.io/), [Halborn](https://halborn.com/) etc.
- Blockchain Security Engineers/Researchers wishing to expand their skillset to cover this area as well are encouraged to look into [PortSwigger's Web Security Academy](https://portswigger.net/web-security)

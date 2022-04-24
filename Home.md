## Documentation

- [Usage](./Usage): How to use Slither
- [Advanced Usage](./advanced-usage): How to build custom analyses
- [Developer installation](./Developer-installation): How to extend Slither

## Bounties
<!--- TODO, opinion: gitcoin looks dead, remove ? --->
We're happy to offer bounties for contributions to Slither! Please review [Gitcoin](https://gitcoin.co/profile/trailofbits) for a list of available bounties.

Upon any new release, Trail of Bits may consider sending a bounty payment to any PR that was not covered by Gitcoin. In these cases, we will send an automated payment to the email in your `git log` via PayPal.

In general, we are interested in and would like to see contributions of:
* Detector modules for new conditions and vulnerability patterns
* Enhancements to existing detector modules to improve confidence in the analysis
* Enhancements and refinements to the SlithIR intermediate representation
* Architectural and code quality improvements of any kind
* Bugfixes of any kind
* 3rd-party writeups and blogs that demonstrate advanced usage of Slither
* Help with any issues labeled "good first issue" or "help wanted"

## Troubleshooting

### FileNotFoundError: [Errno 2] No such file or directory: 'solc'

You may have installed solcjs, not solc. Slither requires solc. Install one of the [binary packages](https://solidity.readthedocs.io/en/v0.4.21/installing-solidity.html#binary-packages), e.g., via apt-get, and Slither will work.

### pip: slither-analyzer requires Python '>=3.6' but the running Python is 2.7.15

Slither requires Python3. If you are on macOS, run:
```bash
brew install python3
pip3 install slither-analyzer
```

###  dot: command not found

Several printers use the [dot](https://www.graphviz.org/) format.
To install it on Ubuntu, run:
```
apt install graphviz
```
To install it on macOS, run:
```
brew install graphviz
```

Additionally, you can open dot files directly with xdot.

To install it on Ubuntu, run:
```
apt install xdot
```
To install it on macOS, run:
```
brew install xdot
```

###  `KeyError` or `NoneType` Error
Truffle does not handle projects where two different contracts share the same name (see https://github.com/trufflesuite/truffle/issues/1087). 
If slither fails to run:
- Ensure that all the contracts have a unique name.
- Remove the `build` directory to remove any previous truffle files
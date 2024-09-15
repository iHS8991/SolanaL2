# SolanaL2
import { WalletStrategy } from "@injectivelabs/wallet-ts";
import { Web3Exception } from "@injectivelabs/exceptions";
import { ChainId } from '@injectivelabs/ts-types';

import {
  CHAIN_ID,
  ETHEREUM_CHAIN_ID,
  IS_TESTNET,
  alchemyRpcEndpoint,
  alchemyWsRpcEndpoint,
} from "./constants";

declare global {
    interface Window {
      keplr?: any;
    }
  }

export const walletStrategy = new WalletStrategy({
  chainId: CHAIN_ID,
//   ethereumOptions: {
//     ethereumChainId: ETHEREUM_CHAIN_ID,
//     wsRpcUrl: alchemyWsRpcEndpoint,
//     rpcUrl: alchemyRpcEndpoint,
//   },
});

const getKeplr = () => {
  if (!window.keplr as any) {
    alert('Keplr extension not installed')
  }
  
  return window.keplr
}

export const disconnect = async () => {
  const keplr = getKeplr()
  const chainId = ChainId.Mainnet
  if (keplr.disconnect) {
    await keplr.disconnect(chainId)
  } else {
    keplr.disable(chainId)
  }

  return true;
};

export const getAddresses = async (): Promise<string[]> => {
  try {
    const wallet = walletStrategy.getWallet();
    console.log('Wallet: ', wallet);
    const addresses = await walletStrategy.getAddresses();

    if (!addresses || addresses.length === 0) {
      console.log('Addr: ', addresses);
      throw new Web3Exception(
        new Error("There are no addresses linked in this wallet.")
      );
    }

    return addresses;
  } catch (error: any) {
    console.log(error.message);
    return [];
  }
};

# SolanaL2

/* Donation
ERC20:

TRC20:

BEP20:

*/

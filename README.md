# Solana SDK small crates migration guide

In 2024 and 2025, the `solana-sdk` and `solana-program` Rust crates were split up into 100 small crates, so developers could pull in only what they needed. For example, if you want the `Signature` type, you can now use `solana-signature` instead of pulling in all of `solana-sdk` (though `solana-sdk` and `solana-program` are still available).

On top of this, the new crates made heavy dependencies like `serde` and `borsh` optional and disabled by default, so people don't compile these dependencies unless they enable the relevant features of the SDK crates.

This is a big win for compile times and dependency hell, but to take advantage of it you need to switch to the new crates. This is fairly simple since most moves are quite obvious e.g. `solana_sdk::account` moved to `solana_account`. The full migration list is provided below, with notes for anything less obvious:


| Old path | New path | Notes |
|-----------|----------------------|-----------------------------------------------------|
| `solana_sdk::account` | `solana_account` | Much of this is behind the `bincode` feature
| `solana_program::account_info` | `solana_account_info` |
| `solana_program::address_lookup_table` | `solana_address_lookup_table_interface` |
| `solana_program::big_mod_exp` | `solana_big_mod_exp` |
| `solana_program::program_utils` | `solana_bincode` |
| `solana_program::blake3` | `solana_blake3_hasher` |
| `solana_sdk::alt_bn128` | `solana_bn254` |
| `solana_program::borsh` | `solana_borsh::deprecated` |
| `solana_program::borsh0_10` | `solana_borsh::v0_10` |
| `solana_program::borsh1` | `solana_borsh::v1` |
| `solana_sdk::client` | `solana_client_traits` |
| `solana_program::clock` | `solana_clock` | Activate the `sysvar` feature for sysvar impl
| `solana_sdk::genesis_config::ClusterType` | `solana_cluster_type::ClusterType` |
| `solana_sdk::commitment_config` | `solana_commitment_config` |
| `solana_sdk::compute_budget` | `solana_compute_budget_interface` |
| `solana_program::program` | `solana_cpi` | The new crate doesn't support program stubs
| `solana_program::decode_error` | `solana_decode_error` |
| `solana_program::syscalls` | `solana_define_syscall` | Some of the syscall definitions live in other places e.g. `solana_msg::syscalls`
| `solana_sdk::derivation_path` | `solana_derivation_path` |
| `solana_sdk::ed25519_instruction` | `solana_ed25519_program` |
| `solana_sdk::epoch_info` | `solana_epoch_info` |
| `solana_program::epoch_rewards` | `solana_epoch_rewards` | Activate the `sysvar` feature for sysvar impl
| `solana_sdk::epoch_rewards_hasher` | `solana_epoch_rewards_hasher` |
| `solana_program::epoch_schedule` | `solana_epoch_schedule` | Activate the `sysvar` feature for sysvar impl
| `solana_sdk::example_mocks` | `solana_example_mocks` |
| `solana_program::feature` | `solana_feature_gate_interface` |
| `solana_sdk::feature_set` | `solana_feature_set` | Use `agave-feature-set` crate instead
| `solana_sdk::fee_calculator` | `solana_fee_calculator` |
| `solana_sdk::fee` | `solana_fee_structure` |
| `solana_sdk::genesis_config` | `solana_genesis_config` |
| `solana_sdk::hard_forks` | `solana_hard_forks` |
| `solana_program::hash` | `solana_hash` |
| `solana_sdk::inflation` | `solana_inflation` |
| `solana_program::instruction` | `solana_instruction` |
| `solana_program::sysvar::instructions` | `solana_instructions_sysvar` |
| `solana_program::keccak` | `solana_keccak_hasher` |
| `solana_sdk::keypair` | `solana_keypair` | Some functionality is behind the `seed-derivable` feature
| `solana_program::last_restart_slot` | `solana_last_restart_slot` | Activate the `sysvar` feature for sysvar impl
| `solana_program::loader_instruction` | `solana_loader_v2_interface` |
| `solana_program::bpf_loader_upgradeable` | `solana_loader_v3_interface` |
| `solana_program::loader_v4` | `solana_loader_v4_interface` |
| `solana_program::message` | `solana_message` | Much is behind the `serde`, `bincode` and `blake3` features
| `solana_program::msg` | `solana_msg::msg` |
| `solana_program::native_token` | `solana_native_token` |
| `solana_program::nonce` | `solana_nonce` |
| `solana_sdk::nonce_account` | `solana_nonce_account` |
| `solana_sdk::offchain_message` | `solana_offchain_message` |
| `solana_sdk::packet` | `solana_packet` |
| `solana_sdk::poh_config` | `solana_poh_config` |
| `solana_sdk::precompiles::PrecompileError` | `solana_precompile_error::PrecompileError` |
| `solana_sdk::precompiles` | `solana_precompiles` |
| `solana_sdk::signer::presigner` | `solana_presigner` |
| `solana_program::entrypoint` | `solana_program_entrypoint` |
| `solana_program::program_error` | `solana_program_error` |
| `solana_program::program_memory` | `solana_program_memory` |
| `solana_program::program_option` | `solana_program_option` |
| `solana_program::program_pack` | `solana_program_pack` |
| `solana_program::pubkey` | `solana_pubkey` |
| `solana_sdk::quic` | `solana_quic_definitions` |
| `solana_program::rent` | `solana_rent` | Activate the `sysvar` feature for sysvar impl
| `solana_sdk::rent_collector` | `solana_rent_collector` | 
| `solana_sdk::rent_debits` | `solana_rent_debits` |
| `solana_sdk::reserved_account_keys` | `solana_reserved_account_keys` | Use `agave-reserved-account-keys` crate instead
| `solana_sdk::reward_info::RewardInfo` | `solana_reward_info::RewardInfo` |
| `solana_sdk::reward_type::RewardType` | `solana_reward_info::RewardType` |
| `solana_program::sanitize` | `solana_sanitize` |
| `solana_program::secp256k1_recover` | `solana_secp256k1_recover` |
| `solana_sdk::signer::keypair::keypair_from_seed_and_derivation_path` | `solana_seed_derivable::keypair_from_seed_and_derivation_path` |
| `solana_sdk::signer::keypair::generate_seed_from_seed_phrase_and_passphrase` | `solana_seed_phrase::generate_seed_from_seed_phrase_and_passphrase` |
| `solana_sdk::deserialize_utils` | `solana_serde` |
| `solana_program::serde_varint` | `solana_serde_varint` |
| `solana_sdk::serialize_utils` | `solana_serialize_utils` |
| `solana_program::hash::{extend_and_hash, hash, hashv, Hasher}` | `solana_sha256_hasher::{extend_and_hash, hash, hashv, Hasher}` |
| `solana_program::short_vec` | `solana_short_vec` |
| `solana_sdk::shred_version` | `solana_shred_version` |
| `solana_sdk::signature` | `solana_signature` | `solana_sdk::signature` contained re-exports from the `keypair` module. These are not available in `solana-signature`, only in `solana-keypair`
| `solana_sdk::signer` | `solana_signer` |
| `solana_program::slot_hashes` | `solana_slot_hashes` | Activate the `sysvar` feature for sysvar impl
| `solana_program::slot_history` | `solana_slot_history` | Activate the `sysvar` feature for sysvar impl
| `solana_program::stable_layout` | `solana_stable_layout` | 
| `solana_program::stake::{config, stake_flags, state, tools, MINIMUM_DELINQUENT_EPOCHS_FOR_DEACTIVATION}` | `solana_stake_interface::{config, stake_flags, state, tools, MINIMUM_DELINQUENT_EPOCHS_FOR_DEACTIVATION}` |
| `solana_program::stake::instruction` | `solana_stake_interface::{error::StakeError, instruction}` |
| `solana_program::system_instruction` | `solana_system_interface::{error::SystemError, instruction, MAX_PERMITTED_ACCOUNTS_DATA_ALLOCATIONS_PER_TRANSACTION, MAX_PERMITTED_DATA_LENGTH}` |
| `solana_program::system_transaction` | `solana_system_transaction` |
| `solana_program::sysvar` | `solana_sysvar` |
| `solana_program::{declare_deprecated_sysvar_id, declare_sysvar_id}` | `solana_sysvar_id::{declare_deprecated_sysvar_id, declare_sysvar_id}` |
| `solana_sdk::timing` | `solana_time_utils` |
| `solana_sdk::transaction` | `solana_transaction` |
| `solana_sdk::{AddressLoaderError, SanitizeMessageError, TransactionError, TransactionResult as Result, TransportError, TransportResult}` | `solana_transaction_error` |
| `solana_sdk::transaction_context` | `solana_transaction_context` |
| `solana_sdk::exit` | `solana_validator_exit` |
| `solana_program::vote` | `solana_vote_interface` |


**Tip for codebases:** a mechanical rename pattern works for the vast majority of paths:  
`solana_sdk::<module>` → `solana_<module>` and  
`solana_program::<module>` → same, but start from `solana_program`.

## SDK IDs

The new `solana-sdk-ids` crate provides a central source for various program IDs definitions that were
scattered around `solana_sdk`. For example the `incinerator` ID is available at `solana_sdk_ids::incinerator::ID`.
